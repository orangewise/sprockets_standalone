require 'rubygems'
require 'bundler'
require 'pathname'
require 'logger'
require 'fileutils'
require 'uglifier'
require 'sass'
require 'asset_sync'

Bundler.require
# File.dirname(__FILE__)
# ROOT        = File.dirname(__FILE__).parent()
ROOT        = Pathname(File.dirname(__FILE__)).parent().join("assets")
LOGGER      = Logger.new(STDOUT)
BUNDLES     = %w( all.css all.js )
BUILD_DIR   = ROOT.join("build")
SOURCE_DIR  = ROOT.join("src")

task :default do
  puts "Try 'bundle exec rake compile'"
end

task :compile do
  sprockets = Sprockets::Environment.new(ROOT) do |env|
    env.js_compressor = :uglifier
    env.css_compressor = :scss
    env.logger = LOGGER
  end
  
  sprockets.append_path(SOURCE_DIR.join('javascripts').to_s)
  sprockets.append_path(SOURCE_DIR.join('stylesheets').to_s)

  BUNDLES.each do |bundle|
    # LOGGER.debug("#{bundle}");
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


task :sync do
  LOGGER.debug("Sync assets with AWS")
  asset_sync = AssetSync

end
