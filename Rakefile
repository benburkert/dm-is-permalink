require 'rubygems'
require 'spec'
require 'spec/rake/spectask'
require 'pathname'
require "spec/rake/spectask"
require 'rake/gempackagetask'

ROOT = Pathname(__FILE__).dirname.expand_path
require ROOT + 'lib/dm-is-permalink/is/version'

AUTHOR = "Brian Smith"
EMAIL  = "wbsmith83@gmail.com"
GEM_NAME = "dm-is-permalink"
GEM_VERSION = DataMapper::Is::Permalink::VERSION
GEM_DEPENDENCIES = [["dm-core", ">= #{GEM_VERSION}"]]
GEM_CLEAN = ["log", "pkg"]
GEM_EXTRAS = { :has_rdoc => true, :extra_rdoc_files => %w[ README LICENSE TODO ] }

PROJECT_NAME = "dm-is-permalink"
PROJECT_URL  = "http://github.com/sam/dm-more/tree/master/dm-is-permalink"
PROJECT_DESCRIPTION = PROJECT_SUMMARY = "DataMapper plugin for adding a permalink to a model"
FILES = %w(LICENSE README Rakefile TODO) + Dir.glob("{lib,spec}/**/*")
task :default => [ :spec ]

WIN32 = (RUBY_PLATFORM =~ /win32|mingw|cygwin/) rescue nil
SUDO  = WIN32 ? '' : ('sudo' unless ENV['SUDOLESS'])

spec = Gem::Specification.new do |s|
  s.name = GEM_NAME
  s.version = GEM_VERSION
  s.platform = Gem::Platform::RUBY
  s.has_rdoc = true
  s.extra_rdoc_files = ["README", "LICENSE", 'TODO']
  s.summary = PROJECT_SUMMARY
  s.description = PROJECT_DESCRIPTION
  s.author = AUTHOR
  s.email = EMAIL
  s.homepage = PROJECT_URL

  s.require_path = 'lib'
  s.autorequire = GEM_NAME
  s.files = FILES
end


desc "Install #{GEM_NAME} #{GEM_VERSION} (default ruby)"
task :install => [ :package ] do
  sh "#{SUDO} gem install --local pkg/#{GEM_NAME}-#{GEM_VERSION} --no-update-sources", :verbose => false
end

desc "Uninstall #{GEM_NAME} #{GEM_VERSION} (default ruby)"
task :uninstall => [ :clobber ] do
  sh "#{SUDO} gem uninstall #{GEM_NAME} -v#{GEM_VERSION} -I -x", :verbose => false
end

namespace :jruby do
  desc "Install #{GEM_NAME} #{GEM_VERSION} with JRuby"
  task :install => [ :package ] do
    sh %{#{SUDO} jruby -S gem install --local pkg/#{GEM_NAME}-#{GEM_VERSION} --no-update-sources}, :verbose => false
  end
end

desc 'Run specifications'
Spec::Rake::SpecTask.new(:spec) do |t|
  t.spec_opts << '--options' << 'spec/spec.opts' if File.exists?('spec/spec.opts')
  t.spec_files = Pathname.glob(Pathname.new(__FILE__).dirname + 'spec/**/*_spec.rb')

  begin
    t.rcov = ENV.has_key?('NO_RCOV') ? ENV['NO_RCOV'] != 'true' : true
    t.rcov_opts << '--exclude' << 'spec'
    t.rcov_opts << '--text-summary'
    t.rcov_opts << '--sort' << 'coverage' << '--sort-reverse'
  rescue Exception
    # rcov not installed
  end
end

Rake::GemPackageTask.new(spec) do |package|
  package.gem_spec = spec
  package.need_zip = true
  package.need_tar = true
end