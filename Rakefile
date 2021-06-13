require "bundler/gem_tasks"

require "rake/testtask"
Rake::TestTask.new(:unit) do |t|
  t.libs.push "lib"
  t.test_files = FileList["spec/**/*_spec.rb"]
  t.verbose = true
end

begin
  require "cucumber"
  require "cucumber/rake/task"
  Cucumber::Rake::Task.new(:features) do |t|
    t.cucumber_opts = ["features", "-x", "--format progress", "--no-color", "--tags ~@ignore"]
  end
rescue LoadError
  puts "cucumber is not available. (sudo) gem install cucumber to run tests."
end

desc "Run all quality tasks"
task quality: [:style]

task default: [:quality]

desc "Display LOC stats"
task :stats do
  puts "\n## Production Code Stats"
  sh "countloc -r lib/kitchen lib/kitchen.rb"
  puts "\n## Test Code Stats"
  sh "countloc -r spec features"
end

begin
  require "chefstyle"
  require "rubocop/rake_task"
  RuboCop::RakeTask.new(:style) do |task|
    task.options += ["--display-cop-names", "--no-color"]
  end
rescue LoadError
  puts "chefstyle is not available. (sudo) gem install chefstyle to do style checking."
end
