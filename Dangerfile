# Sometimes it's a README fix, or something like that - which isn't relevant for
# including in a project's CHANGELOG for example
#declared_trivial = github.pr_title.include? "#trivial"

# Make it more obvious that a PR is a work in progress and shouldn't be merged yet
#warn("PR is classed as Work in Progress") if github.pr_title.include? "[WIP]"

# Warn when there is a big PR
#warn("Big PR") if git.lines_of_code > 500

# Don't let testing shortcuts get into master by accident
#fail("fdescribe left in tests") if `grep -r fdescribe specs/ `.length > 1
#fail("fit left in tests") if `grep -r fit specs/ `.length > 1

#android_lint.report_file = "app/build/reports/lint-results.xml"
#android_lint.lint


#apkanalyzer.apk_file = "app/build/outputs/apk/debug/app-debug.apk"
#apkanalyzer.dex_references

require 'open3'
require 'shellwords'

system "./gradlew assembleDebug"
o, e, s = Open3.capture3("apkanalyzer -h dex references app/build/outputs/apk/debug/app-debug.apk")
if s.success?
  message = "#### Number of method references\n\n"
  message << "| Dex file | count |\n"
  o.each_line do |line|
    v = line.chomp.split(" ")
    message << "| --- | --- |\n"
    message << "| #{v[0]} | #{v[1]} |"
  end
  markdown(message)
else
  fail(e)
end

o, e, s = Open3.capture3("apkanalyzer -h apk file-size app/build/outputs/apk/debug/app-debug.apk")
if s.success?
  message = "#### APK file size\n\n"
  message << "| size |\n"
  message << "| --- |\n"
  message << "| #{o.chomp} |\n"
  markdown(message)
else
  fail(e)
end
