require 'rubygems'
Gem::manage_gems
require 'rake/gempackagetask'

kind = Config::CONFIG['DLEXT']

spec = Gem::Specification.new do |s|
   s.platform = Gem::Platform::CURRENT
   s.name = 'ruby-serialport'
   s.version = "0.7.0"
   s.summary = "Ruby/SerialPort is a Ruby library that provides a class for using RS-232 serial ports."

   s.files = Dir.glob("{doc,ext,lib,test}/**/*").delete_if { |item| item.include?( ".svn" ) }
   s.files.concat [ "README", "CHANGELOG" ]
   s.extensions << 'Rakefile'
   s.has_rdoc = true
   s.extra_rdoc_files = [ "README", "ext/native/serialport.c", "ext/native/serialport.h" ]
   s.rdoc_options = [ "--main", "README" ]
   s.authors = ["Guillaume Pierronnet", "Alan Stern", "Daniel E. Shipton"]
   s.email = "daniel.shipton.oss@gmail.com"
   s.homepage = "http://ruby-serialport.rubyforge.org"
end

Rake::GemPackageTask.new(spec) do |pkg|
   pkg.need_tar = true
end

task :default => "pkg/#{spec.name}-#{spec.version}-#{spec.platform}.gem" do
   puts " #{spec.name} => pkg/#{spec.name}-#{spec.version}-#{spec.platform}.gem generated"
end

task :clean do
  rm_rf(Dir['ext/native/*.{o,so,bundle,dylib,log}'], :verbose => true)
  rm_rf(Dir['ext/native/Makefile'], :verbose => true)
  rm_rf(Dir['pkg'], :verbose => true)
end

task :extension => ["ext/native/serialport.#{kind}"]

file "ext/native/serialport.#{kind}" => ['ext/native/Makefile'] do |t|
  Dir.chdir('ext/native') { sh 'make' }
end

file 'ext/native/Makefile' do |t|
  Dir.chdir('ext/native') { sh 'ruby extconf.rb' }
end

Rake::Task[:test].prerequisites << :extension
Rake::Task[:default].prerequisites << :extension
