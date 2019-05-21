require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'

PuppetLint.configuration.log_format = '%{path}:%{line}:%{check}:%{KIND}:%{message}'
PuppetLint.configuration.fail_on_warnings = true
PuppetLint.configuration.send('disable_80chars')
PuppetLint.configuration.send('disable_140chars')
PuppetLint.configuration.send('disable_class_inherits_from_params_class')
PuppetLint.configuration.send('disable_selector_inside_resource')
PuppetLint.configuration.send('disable_only_variable_string')

exclude_paths = %w(
  spec/**/*
  serverspec/**/*
  pkg/**/*
  examples/**/*
  vendor/**/*
  .vendor/**/*
)

PuppetLint::RakeTask.new :lint do |config|
  config.ignore_paths = exclude_paths
end

PuppetSyntax.exclude_paths = exclude_paths

desc 'Run validate, parallel_spec, lint'
task test: %w(release_checks)

namespace :check do
  desc 'Check for trailing whitespace'
  task :trailing_whitespace do
    Dir.glob('**/*.md', File::FNM_DOTMATCH).sort.each do |filename|
      next if filename =~ %r{^((modules|acceptance|\.?vendor|spec/fixtures|pkg)/|REFERENCE.md)}
      File.foreach(filename).each_with_index do |line, index|
        if line =~ %r{\s\n$}
          puts "#{filename} has trailing whitespace on line #{index + 1}"
          exit 1
        end
      end
    end
  end
end
Rake::Task[:release_checks].enhance ['check:trailing_whitespace']
