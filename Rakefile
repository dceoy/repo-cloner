# coding: utf-8
# Rakefile for repo-cloner

require 'yaml'
require 'fileutils'

CONFIG_YML = 'config.yml'
PROMPT = '>>>'

def load_config(yml)
  y = YAML.load_file(yml)
  protocol = y['protocol']
  dir = y['dir']
  repos = y['repos'].map {|r| r + '.git'}

  case protocol
  when 'ssh' then
    return({
      'dir' => dir,
      'repos' => repos.map {|r| [r, 'git@github.com:' + r]}.to_h
    })
  when 'https' then
    return({
      'dir' => dir,
      'repos' => repos.map {|r| [r, 'https://github.com/' + r]}.to_h
    })
  else
    raise "invalid protocol: #{protocol}"
  end
end


task :default => :mirror

desc 'mirror repositories on GitHub'
task :mirror do
  puts "#{PROMPT} read ./#{CONFIG_YML}"
  if File.exists?(CONFIG_YML)
    cf = load_config(CONFIG_YML)
    repo_dir = cf['dir']
    h_repo = cf['repos']
  else
    raise "#{CONFIG_YML} not found"
  end

  h_repo.keys.map {|k| k.split('/')[0]}.to_set.each do |u|
    ud = "#{repo_dir}/#{u}"
    if not Dir.exists?(ud)
      puts "#{PROMPT} make ./#{ud}/"
      FileUtils.mkdir_p(ud)
    end
  end

  h_repo.each do |k, v|
    puts "#{PROMPT} mirror #{v}"
    wd = "#{repo_dir}/#{k}"
    if Dir.exists?(wd)
      puts "#{PROMPT} fetch updates for #{k}"
      sh "git --git-dir=\"#{wd}\" fetch --all"
    else
      puts "#{PROMPT} clone #{k}"
      sh "git clone --bare #{v} #{wd}"
    end
  end
end

desc 'generate a template for config.yml'
file :template do
  puts "#{PROMPT} generate #{CONFIG_YML}"
  File.open(CONFIG_YML, 'w') do |f|
    f.write({
      'protocol' => 'ssh',
      'dir' => './repos',
      'repos' => ['dceoy/repo-cloner']
    }.to_yaml)
  end
end
