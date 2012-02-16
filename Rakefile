require "rubygems"
require "rdoc/task"
require "rubygems/package_task"
require "rake/testtask"
require "rake/clean"
require "rubygems"

# Some definitions that you'll need to edit in case you reuse this
# Rakefile for your own project.

SHORTNAME     ='iso7816'  # this should be the rubyforge project name
DESC          ='ruby smartcard access'
PKG_VERSION   ='0.0.3'
LONG_DESC  = <<END_DESC
  Utilities to provided ISO 7816 smartcard functionality  
END_DESC
RUBYFORGE_USER  ='a2800276'

# Specifies the default task to execute. 
task  :default => [:test]

# The directory to generate +rdoc+ in.
RDOC_DIR="doc/html"

# This global variable contains files that will be erased by the `clean` task.
# The `clean` task itself is automatically generated by requiring `rake/clean`.

CLEAN << RDOC_DIR << "pkg"


# This is the task that generates the +rdoc+ documentation from the
# source files. Instantiating Rake::RDocTask automatically generates a
# task called `rdoc`.

Rake::RDocTask.new do |rd|
  # Options for documenation generation are specified inside of
  # this block. For example the following line specifies that the
  # content of the README file should be the main page of the
  # documenation.
  rd.main = "README" 
  
  # The following line specifies all the files to extract
  # documenation from.
  rd.rdoc_files.include(  "README", "AUTHORS", "LICENSE", "TODO",
        "CHANGELOG", "bin/**/*", "lib/**/*.rb", 
        "examples/**/*rb","test/**/*.rb", "doc/*.rdoc")
  # This one specifies the output directory ...
  rd.rdoc_dir   = "doc/html"

  # Or the HTML title of the generated documentation set.
  rd.title      = "#{SHORTNAME}: #{DESC}"

  # These are options specifiying how source code inlined in the
  # documentation should be formatted.
  
  rd.options    = ["--line-numbers"]

  # Check:
  # `rdoc --help` for more rdoc options
  # the {rdoc documenation home}[http://www.ruby-doc.org/stdlib/libdoc/rdoc/rdoc/index.html]
  # or the documentation for the +Rake::RDocTask+ task[http://rake.rubyforge.org/classes/Rake/RDocTask.html]
end

# The GemPackageTask facilitates getting all your files collected
# together into gem archives. You can also use it to generate tarball
# and zip archives.

# First you'll need to assemble a gemspec

PKG_FILES   = FileList['lib/**/*.rb', 'bin/**/*', 'examples/**/*', '[A-Z]*', 'test/**/*'].to_a

spec = Gem::Specification.new do |s|
  s.platform  = Gem::Platform::RUBY
  s.summary   = "#{SHORTNAME}: #{DESC}"
  s.name      = SHORTNAME
  s.version   = PKG_VERSION
  s.files     = PKG_FILES
  s.author    = "Tim Becker"
  s.email     = "tim.becker@kuriositaet.de"
  s.homepage  = "https://github.com/a2800276/7816"
  s.requirements << "none"
  s.require_path = 'lib'
  s.add_dependency("hexy")
  s.add_dependency("tlv")
  s.add_dependency("smartcard")
  #s.has_rdoc=true
  s.description = LONG_DESC
end

# Adding a new GemPackageTask adds a task named `package`, which generates
# packages as gems, tarball and zip archives.
Gem::PackageTask.new(spec) do |pkg|
  pkg.need_zip = true
  pkg.need_tar_gz = true
end


# This task is used to demonstrate how to upload files to Rubyforge.
# Calling `upload_page` creates a current version of the +rdoc+
# documentation and uploads it to the Rubyforge homepage of the project,
# assuming it's hosted there and naming conventions haven't changed.
#
# This task uses `sh` to call the `scp` binary, which is plattform
# dependant and may not be installed on your computer if you're using
# Windows. I'm currently not aware of any pure ruby way to do scp
# transfers.

RubyForgeProject=SHORTNAME

desc "Upload the web pages to the web."
task :upload_pages => ["rdoc"] do
  if RubyForgeProject then
    path = "/var/www/gforge-projects/#{RubyForgeProject}"
    sh "scp -r doc/html/* #{RUBYFORGE_USER}@rubyforge.org:#{path}"
    sh "scp doc/images/*.png #{RUBYFORGE_USER}@rubyforge.org:#{path}/images"
  end
end

# This task will run the unit tests provided in files called
# `test/test*.rb`. The task itself can be run with a call to `rake test`

Rake::TestTask.new do |t| 
  t.libs << "test" 
  t.libs << "lib" 
  t.ruby_opts = ["-rubygems"]
  t.test_files = FileList['test/*.rb'] 
  t.verbose = true 
end


desc "generate iso apdu command classes"
task :iso_apdu do
  ruby "-rubygems build/create_apdu.rb -i lib/apdu.spec -o lib/apdu_generated.rb -s lib/apdu_generated_impl.rb"
end

desc "delete generated apdu classes"
task :iso_apdu_clean do
  File.delete "lib/apdu_generated.rb" if File.exist? "lib/apdu_generated.rb"
end



