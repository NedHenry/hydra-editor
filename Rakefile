#!/usr/bin/env rake
begin
  require 'bundler/setup'
rescue LoadError
  puts 'You must `gem install bundler` and `bundle install` to run rake tasks'
end
begin
  require 'rdoc/task'
rescue LoadError
  require 'rdoc/rdoc'
  require 'rake/rdoctask'
  RDoc::Task = Rake::RDocTask
end

RDoc::Task.new(:rdoc) do |rdoc|
  rdoc.rdoc_dir = 'rdoc'
  rdoc.title    = 'HydraEditor'
  rdoc.options << '--line-numbers'
  rdoc.rdoc_files.include('README.rdoc')
  rdoc.rdoc_files.include('lib/**/*.rb')
end


Bundler::GemHelper.install_tasks

dummy = File.expand_path('../spec/dummy', __FILE__)
rakefile = File.join(dummy, 'Rakefile')

task :clean do
  sh "rm -rf #{dummy}"
end

task :setup do
  unless File.exists?("#{dummy}/Rakefile")
    `rails new #{dummy}`
    `cp -r spec/support/Gemfile #{dummy}/`
    `cp -r spec/support/lib/generators #{dummy}/lib`
    Dir.chdir(dummy) do
      sh "rails g blacklight --devise"
      sh "rails g hydra:head -f"
      sh "rm spec/models/user_spec.rb" # generated by devise
      puts "Generating test app"
      sh "rails generate test_app"
      puts "running migrations"
      sh 'rake db:migrate db:test:prepare'
    end
  end
end

task :spec => :setup do
  here = File.expand_path("../", __FILE__)
  Dir.chdir(dummy) do
    Bundler.with_clean_env do
      sh "bundle exec rspec --color -I#{here}/spec #{here}/spec"
    end
  end
end