NAME='openresty'
DESCRIPTION='Minimal openresty package for xenial, created to illustrate nginx consul integration'
VERSION='0.1.5'
ITERATION=1
BUILD_DIR='./build'
OUT_DIR='./pkg'
PREFIX='/usr/lib/openresty'

# Setup RUBY ENV
ENV['PATH'] =  "#{Dir.pwd}/.gems/bin:#{ENV['PATH']}"
ENV['GEM_HOME'] = "#{Dir.pwd}/.gems"
ENV['GEM_PATH'] = '/var/lib/gems/2.3.0'

desc 'Install Dependencies'
task :install_deps do
  Dir.mkdir "#{Dir.pwd}/.gems" unless File.directory? "#{Dir.pwd}/.gems"
  sh %{ bundle update }
end

desc 'Clean'
task :clean do
  sh %{ rm -rf lua-resty-core }
  sh %{ rm -rf lua-resty-http }
  sh %{ rm -rf lua-resty-iputils }
  sh %{ rm -rf lua-resty-lrucache }
  sh %{ rm -rf ./build }
  sh %{ rm -rf ./pkg }
end

desc 'Fetch Source'
task :fetch do
  sh %{ git clone https://github.com/openresty/lua-resty-core }
  sh %{ git clone https://github.com/pintsized/lua-resty-http }
  sh %{ git clone https://github.com/hamishforbes/lua-resty-iputils }
  sh %{ git clone https://github.com/openresty/lua-resty-lrucache }
end

desc 'Build'
task :build do
  # Reset revisions to known working copies (for xenial nginx-extras package)
  sh %{ cd lua-resty-core; git checkout -q 87e46ae64c26495cc73c6f045f706902527c7b63; git reset --hard }
  sh %{ cd lua-resty-http; git checkout -q tags/v0.07; git reset --hard }
  sh %{ cd lua-resty-lrucache; git checkout -q tags/v0.04; git reset --hard }
  sh %{ mkdir -p #{BUILD_DIR}/usr/lib/openresty }
  sh %{ mkdir #{BUILD_DIR}#{PREFIX}/lua-resty-core }
  sh %{ mkdir #{BUILD_DIR}#{PREFIX}/lua-resty-http }
  sh %{ mkdir #{BUILD_DIR}#{PREFIX}/lua-resty-iputils }
  sh %{ mkdir #{BUILD_DIR}#{PREFIX}/lua-resty-lrucache }
  sh %{ cp -Rp ./lua-resty-core/lib #{BUILD_DIR}#{PREFIX}/lua-resty-core }
  sh %{ cp -Rp ./lua-resty-core/README.markdown #{BUILD_DIR}#{PREFIX}/lua-resty-core }
  sh %{ cp -Rp ./lua-resty-http/lib #{BUILD_DIR}#{PREFIX}/lua-resty-http }
  sh %{ cp -Rp ./lua-resty-http/LICENSE #{BUILD_DIR}#{PREFIX}/lua-resty-http }
  sh %{ cp -Rp ./lua-resty-iputils/lib #{BUILD_DIR}#{PREFIX}/lua-resty-iputils }
  sh %{ cp -Rp ./lua-resty-iputils/README.markdown #{BUILD_DIR}#{PREFIX}/lua-resty-iputils }
  sh %{ cp -Rp ./lua-resty-lrucache/lib #{BUILD_DIR}#{PREFIX}/lua-resty-lrucache }
  sh %{ cp -Rp ./lua-resty-lrucache/README.markdown #{BUILD_DIR}#{PREFIX}/lua-resty-lrucache }
end

desc 'Package'
task :pkg do
  sh %{ mkdir ./pkg }
  sh %{
    fpm -t deb \
              -s dir \
              -n #{NAME} \
              -v #{VERSION} \
              --iteration #{ITERATION} \
              --description "#{DESCRIPTION}" \
              -a all \
              --deb-no-default-config-files \
              --deb-user root \
              --deb-group root \
              -p #{OUT_DIR} \
              -C #{BUILD_DIR} \
              .
  }
end

task :default => [:install_deps, :clean, :fetch, :build, :pkg]

