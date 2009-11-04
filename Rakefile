desc "deploy site to aconcagua"
task :deploy_old do
  require 'rubygems'
  require 'highline/import'
  require 'net/ssh'

  branch = "master"

  username = ask("Username:  ") { |q| q.echo = true }
  password = ask("Password:  ") { |q| q.echo = "*" }

  Net::SSH.start('aconcagua', nil) do |ssh|
    commands = <<EOF
cd ~/litanyagainstfear/cached-copy
git checkout #{branch}
git pull origin #{branch}
git checkout -f
rm -rf _site
jekyll --no-auto
mv _site ../_#{branch}
mv ../#{branch} _old
mv ../_#{branch} ../#{branch}
rm -rf _old
EOF
    commands = commands.gsub(/\n/, "; ")
    ssh.exec commands
  end
end

# Adopted from Scott Kyle's Rakefile
# http://github.com/appden/appden.github.com/blob/master/Rakefile

def jekyll(opts = '')
  sh 'rm -rf _site'
  # temporary measure until I sort this out (why doesn't it add it to executable path automatically?)
  sh '/opt/ruby-enterprise-1.8.6-20090610/lib/ruby/gems/1.8/gems/mojombo-jekyll-0.5.4/bin/jekyll ' + opts
end

desc 'Build site using Jekyll'
task :build do
  jekyll('--no-auto')
end

desc 'Start server on http://localhost:4000'
task :server do
  jekyll('--server --auto')
end

desc 'Build and deploy to production'
task :deploy => :build do
  sh 'rsync -rtzh --progress --delete _site/ aconcagua:/aconcagua/simplepay_blog/current/'
end
