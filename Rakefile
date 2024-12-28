# coding: utf-8
# Rakefile for repo-cloner

require 'json'
require 'net/http'
require 'uri'
require 'yaml'

CONFIG_YML = 'config.yml'

def console_log(msg)
  puts ">>> #{msg}"
end

def load_config(yml)
  y = YAML.load_file(yml)

  users = y['repos'].grep(/^[^\/]+$/)
  user_repos = y['repos'] - users
  repos = users.map{
    |u| JSON.parse(
      Net::HTTP.get(URI.parse("https://api.github.com/users/#{u}/repos"))
    ).map{|h| "#{u}/#{h['name']}"}
  }.flatten + user_repos

  users = y['repos'].grep(/^[^\/]+$/)
  user_repos = y['repos'] - users
  page = 1
  repos = users.map do |u|
    loop do
      uri = URI.parse("https://api.github.com/users/#{u}/repos?type=all&per_page=100&page=#{page}")
      request = Net::HTTP::Get.new(uri)
      response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
        http.request(request)
      end
      current_repos = JSON.parse(response.body)
      break if current_repos.empty?
      user_repos += current_repos.map { |r| "#{u}/#{r['name']}" }
      page += 1
      sleep 1
    end
    user_repos
  end.flatten

  case y['protocol']
  when 'ssh' then
    return({
      'dir' => y['dir'],
      'repos' => repos.map{|r| ["#{r}.git", "git@github.com:#{r}.git"]}.to_h
    })
  when 'https' then
    return({
      'dir' => y['dir'],
      'repos' => repos.map{|r| ["#{r}.git", "https://github.com/#{r}.git"]}.to_h
    })
  else
    raise "The protocol is invalid: #{y['protocol']}"
  end
end

desc 'mirror repositories on GitHub'
task :default => :mirror

desc 'generate a template for config.yml'
task :init => CONFIG_YML

file CONFIG_YML do
  console_log "Generate #{CONFIG_YML}"
  File.open(CONFIG_YML, 'w') do |f|
    f.write({
      'protocol' => 'ssh',
      'dir' => './repos',
      'repos' => ['dceoy/repo-cloner', 'ruby/rake']
    }.to_yaml)
  end
end

task :mirror => CONFIG_YML do
  console_log "Retrieve repositories from ./#{CONFIG_YML}"
  cf = load_config(CONFIG_YML)
  puts cf['repos'].keys.to_yaml

  cf['repos'].each do |k, v|
    wd = File.join(cf['dir'], k)
    if Dir.exist?(wd)
      console_log "Fetch updates for #{k}"
      sh "git --git-dir='#{wd}' fetch --all"
    else
      console_log "Clone #{k}"
      mkdir_p wd.sub(/\/[^\/]+$/, '')
      sh "git clone --bare #{v} #{wd}"
    end
  end
end
