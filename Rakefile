require "bundler/gem_tasks"
require "rake/testtask"

VERSIONS = %w[2.2.2 2.3.0 2.4.0 2.5.0]

Rake::TestTask.new(:test) do |t|
  t.libs << "test" << "test/lib"
  t.libs << "lib"
  t.test_files = FileList["test/**/test_*.rb"]
end

require 'rake/extensiontask'
spec = eval(File.read('stringio.gemspec'), nil, 'stringio.gemspec')
spec.files.delete_if {|n| %r'\Aext/' =~ n}
spec.extensions.clear
spec.require_paths.insert(0, *%w[stub])
Rake::ExtensionTask.new("stringio", spec) do |ext|
  ext.cross_compile = true
  ext.cross_platform = %w[x86-mingw32 x64-mingw32]
  ext.cross_compiling do |s|
    s.files.concat VERSIONS.map {|v| "lib/#{v[/\A\d+\.\d+/]}/stringio.so"}
  end
end

desc "Compile binaries for mingw platform using rake-compiler-dock"
task 'build:mingw' do
  require 'rake_compiler_dock'
  RakeCompilerDock.sh "bundle && rake cross native gem RUBY_CC_VERSION=#{VERSIONS.join(':')}"
end

task :default => [:compile, :test]
