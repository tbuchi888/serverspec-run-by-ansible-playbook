require 'rake'
require 'rspec/core/rake_task'
require 'yaml'

hostvars = YAML.load_file(ENV['HOST_VARS_PATH'])

namespace :serverspec do
  desc "Run serverspec for #{hostvars["spec_role"]}"
  RSpec::Core::RakeTask.new(hostvars["spec_role"].to_sym) do |t|
    t.pattern = "spec/#{hostvars["spec_role"]}/*_spec.rb"
  end
end
