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
  puts "'bundle exec rake compile_and_sync environment=dev'"
  puts "or"
  puts "'bundle exec rake sprockets:compile environment=dev'"
  puts "or"
  puts "'bundle exec rake sync:assets environment=dev'"
  puts "----------------------------------------------"
end



task :compile_and_sync => ['sprockets:compile', 'sync:assets']

namespace :sprockets do

  task :compile => ['compile_scripts', 'process_images']

  task :compile_scripts do
    puts "Compile scripts"
  
    sprockets = Sprockets::Environment.new(ROOT) do |env|
      env.js_compressor = :uglifier
      env.css_compressor = :scss
      env.logger = LOGGER
    end

    puts "Process javascripts and stylesheets"
    
    sprockets.append_path(SOURCE_DIR.join('javascripts').to_s)
    sprockets.append_path(SOURCE_DIR.join('stylesheets').to_s)
    BUNDLES.each do |bundle|
      puts "bundle: #{bundle}"
      assets = sprockets.find_asset(bundle)
      prefix, basename = assets.pathname.to_s.split('/')[-2..-1]
      FileUtils.mkpath BUILD_DIR.join(prefix)
    
      assets.write_to(BUILD_DIR.join(prefix, basename))
    end
  end  
    
  task :process_images do
    puts "Process images..."
    
    images = Sprockets::Environment.new(ROOT)
    images.append_path(SOURCE_DIR.join('images').to_s)

    images.each_file do |image|
      puts "image: #{image}"
      asset = images.find_asset(image)
      
      prefix, basename = asset.pathname.to_s.split('/')[-2..-1]
      FileUtils.mkpath BUILD_DIR.join(prefix)

      asset.write_to(BUILD_DIR.join(prefix, basename))
    end
  end

end

# bundle exec sync:assets environment:dev

namespace :sync do
    
  task :assets do
    puts "Syncing assets"
    
    puts "environment is set to #{TARGET_ENV}"
    puts "existing_remote_files is set to #{YML[TARGET_ENV]['existing_remote_files']}"

    AssetSync.configure do |config|
      config.fog_provider = YML[TARGET_ENV]['fog_provider']
      config.fog_directory = YML[TARGET_ENV]['fog_directory']
      config.aws_access_key_id = YML[TARGET_ENV]['aws_access_key_id']
      config.aws_secret_access_key = YML[TARGET_ENV]['aws_secret_access_key']
      config.gzip_compression = YML[TARGET_ENV]['gzip_compression']
      config.existing_remote_files = YML[TARGET_ENV]['existing_remote_files']
      config.prefix = TARGET_ENV
      config.public_path = ROOT
    end
    
    puts "start sync..."
    AssetSync.sync
    puts "done"    
  end

end
