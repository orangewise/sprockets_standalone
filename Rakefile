require 'rubygems'
require 'bundler'
require 'pathname'
require 'logger'
require 'fileutils'
require 'uglifier'
require 'sass'
require 'asset_sync'
require 'yaml'

Bundler.require
# File.dirname(__FILE__)
# ROOT        = File.dirname(__FILE__).parent()
ROOT        = Pathname(File.dirname(__FILE__)).parent().join("assets")
LOGGER      = Logger.new(STDOUT)
BUNDLES     = %w( all.css all.js )
TARGET_ENV  = ENV['environment']||"dev"
BUILD_DIR   = ROOT.join("#{TARGET_ENV}/build")
SOURCE_DIR  = ROOT.join("src")

ASSET_SYNC_YML = Pathname(File.dirname(__FILE__)).join("config/asset_sync.yml")
YML = YAML.load_file(ASSET_SYNC_YML)

task :default do
  puts "----------------------------------------------"
  puts "Try: "
  puts "'bundle exec rake sprockets:compile environment=dev'"
  puts "or"
  puts "'bundle exec rake sync:assets environment=dev'"
  puts "----------------------------------------------"
end

namespace :sprockets do

  task :compile do
    puts "compile"
  
    sprockets = Sprockets::Environment.new(ROOT) do |env|
      env.js_compressor = :uglifier
      env.css_compressor = :scss
      env.logger = LOGGER
    end
  
    sprockets.append_path(SOURCE_DIR.join('javascripts').to_s)
    sprockets.append_path(SOURCE_DIR.join('stylesheets').to_s)

    BUNDLES.each do |bundle|
      puts "bundle: #{bundle}"
      assets = sprockets.find_asset(bundle)
      prefix, basename = assets.pathname.to_s.split('/')[-2..-1]
      FileUtils.mkpath BUILD_DIR.join(prefix)

      assets.write_to(BUILD_DIR.join(prefix, basename))

      # assets.to_a.each do |asset|
      #   # strip filename.css.foo.bar.css multiple extensions
      #   realname = asset.pathname.basename.to_s.split(".")[0..1].join(".")
      #   asset.write_to(BUILD_DIR.join(prefix, realname))
      # end
    end
  end

end

# bundle exec sync:assets environment:dev

namespace :sync do
    
  task :assets do
    p "environment is set to #{TARGET_ENV}"

    AssetSync.configure do |config|
      config.fog_provider = YML[TARGET_ENV]['fog_provider']
      config.fog_directory = YML[TARGET_ENV]['fog_directory']
      config.aws_access_key_id = YML[TARGET_ENV]['aws_access_key_id']
      config.aws_secret_access_key = YML[TARGET_ENV]['aws_secret_access_key']
      config.gzip_compression = YML[TARGET_ENV]['gzip_compression']
      config.prefix = TARGET_ENV
      config.public_path = ROOT
    end
    AssetSync.sync
    
  end

end
