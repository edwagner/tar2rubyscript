require 'rubygems'
require 'rake'

desc "Build the realstuff.rb file for the gem."
task :build_realstuff do
  # Copy all of the files to be archived in realstuff.rb into a temporary folder
  # (so that we don't get a lot of extraneous files)
  files_to_copy = Dir.glob("ev/**") +
    %w{CHANGELOG LICENSE README.rdoc SUMMARY VERSION init.rb tar.exe tarrubyscript.rb}
  sourcedir = File.dirname(File.expand_path(__FILE__))
  tempdir = "#{Dir.tmpdir}/tar2rubyscript.gem.#{Process.pid}"
  begin
    FileUtils.rm_rf(tempdir) if File.exists?(tempdir)
    Dir.mkdir(tempdir)
    files_to_copy.each do |f|
      src = File.join(sourcedir, f)
      dest = File.join(tempdir, f)
      FileUtils.mkdir_p(File.dirname(dest))
      FileUtils.copy(src, dest)
    end
    # There's got to be a better way than doing this as a separate process.
    # (init.rb expects ARGV to be set up a certain way,
    # but mucking with ARGV seems potentially dangerous in the context of a rake task)
    system "ruby #{File.join(tempdir, 'init.rb')} #{tempdir} #{File.join(sourcedir, 'realstuff.rb')}"
  ensure
    FileUtils.rm_rf(tempdir) if File.exists?(tempdir)
  end
end

begin
  require 'jeweler'
  Jeweler::Tasks.new do |gem|
    # gem is a Gem::Specification... see http://www.rubygems.org/read/chapter/20 for additional settings
    gem.name = "tar2rubyscript"
    gem.summary = %Q{A Tool for Distributing Ruby Applications}
    gem.description = %Q{Compile ruby applications into a single ruby script file for easy deployment. A simple update of http://www.erikveen.dds.nl/tar2rubyscript}
    gem.email = "ed@edwagner.org"
    gem.homepage = "http://github.com/edwagner/tar2rubyscript"
    gem.authors = ["Erik Veenstra", "Ryan Booker", "Ed Wagner"]
    gem.has_rdoc = false
    gem.bindir = 'bin'
    gem.executables = ['tar2rubyscript']
    gem.files = FileList[
                         "README.rdoc",
                         "SUMMARY",
                         "VERSION",
                         "bin/**",
                         "lib/**",
                         "realstuff.rb"
                        ]
  end
rescue LoadError
  puts "Jeweler (or a dependency) not available. Install it with: sudo gem install jeweler"
end

task :build => :build_realstuff
task 'gemspec:generate' => :build_realstuff
