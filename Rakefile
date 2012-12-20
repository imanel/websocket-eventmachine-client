require 'bundler'
Bundler::GemHelper.install_tasks

# require 'rspec/core/rake_task'

# RSpec::Core::RakeTask.new do |t|
#   t.rspec_opts = ["-c", "-f progress"]
#   t.pattern = 'spec/**/*_spec.rb'
# end

# task :default => :spec

desc "Run autobahn tests for server"
task :autobahn do
  system('wstest --mode=fuzzingclient --spec=autobahn.json')
end
