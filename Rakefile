# coding: utf-8
# Rakefile for repo-cloner

require 'yaml'

CONFIG_YML = 'config.yml'

def console_log(msg)
  puts ">>> #{msg}"
end

def load_config(yml)
  y = YAML.load_file(yml)

  case y['protocol']
  when 'ssh' then
    return({
      'dir' => y['dir'],
      'repos' => y['repos'].map {|r| ["#{r}.git", "git@github.com:#{r}.git"]}.to_h
    })
  when 'https' then
    return({
      'dir' => y['dir'],
      'repos' => y['repos'].map {|r| ["#{r}.git", "https://github.com/#{r}.git"]}.to_h
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
  console_log "generate #{CONFIG_YML}"
  File.open(CONFIG_YML, 'w') do |f|
    f.write({
      'protocol' => 'ssh',
      'dir' => './repos',
      'repos' => ['dceoy/repo-cloner', 'ruby/rake']
    }.to_yaml)
  end
end

task :mirror => CONFIG_YML do
  console_log "read ./#{CONFIG_YML}"
  cf = load_config(CONFIG_YML)

  cf['repos'].each do |k, v|
    wd = File.join(cf['dir'], k)
    if Dir.exists?(wd)
      console_log "fetch updates for #{k}"
      sh "git --git-dir='#{wd}' fetch --all"
    else
      console_log "clone #{k}"
      mkdir_p wd.sub(/\/[^\/]+$/, '')
      sh "git clone --bare #{v} #{wd}"
    end
  end
end
