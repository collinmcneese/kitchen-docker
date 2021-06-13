require "bundler/gem_tasks"
require "cane/rake_task"
require "tailor/rake_task"

desc "Run cane to check quality metrics"
Cane::RakeTask.new do |cane|
  cane.canefile = "./.cane"
end

Tailor::RakeTask.new

desc "Display LOC stats"
task :stats do
  puts "\n## Production Code Stats"
  sh "countloc -r lib"
end

desc "Run all quality tasks"
task quality: [:style]

task default: [:quality]

begin
  require "chefstyle"
  require "rubocop/rake_task"
  RuboCop::RakeTask.new(:style) do |task|
    task.options += ["--display-cop-names", "--no-color"]
  end
rescue LoadError
  puts "chefstyle is not available. (sudo) gem install chefstyle to do style checking."
end

# Create the spec task.
require "rspec/core/rake_task"
RSpec::Core::RakeTask.new(:spec, :tag) do |t, args|
  t.rspec_opts = [].tap do |a|
    a << "--color"
    a << "--format #{ENV["CI"] ? "documentation" : "Fuubar"}"
    a << "--backtrace" if ENV["VERBOSE"] || ENV["DEBUG"]
    a << "--seed #{ENV["SEED"]}" if ENV["SEED"]
    a << "--tag #{args[:tag]}" if args[:tag]
    a << "--default-path test"
    a << "-I test/spec"
  end.join(" ")
end
