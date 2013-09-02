
##############################
# for testing 
#
# NB: use rake -I ../world.db.ruby/lib -I ./lib sportdb:build


BUILD_DIR = "./build"

# -- output db config
SPORT_DB_PATH = "#{BUILD_DIR}/sport.db"

# -- input repo sources config
WORLD_DB_INCLUDE_PATH = '../world.db'
SPORT_DB_INCLUDE_PATH = '../openfootball/world-cup'   # todo: make it into an array


DB_CONFIG = {
  adapter:    'sqlite3',
  database:   SPORT_DB_PATH
}

directory BUILD_DIR

task :clean do
  rm SPORT_DB_PATH if File.exists?( SPORT_DB_PATH )
end

task :env => BUILD_DIR do
  require 'worlddb'   ### NB: for local testing use rake -I ./lib dev:test e.g. do NOT forget to add -I ./lib
  require 'sportdb'
  require 'logutils/db'

  LogUtils::Logger.root.level = :info

  pp DB_CONFIG
  ActiveRecord::Base.establish_connection( DB_CONFIG )
end

task :create => :env do
  LogDb.create
  WorldDb.create
  SportDb.create
end
  
task :importworld => :env do
  WorldDb.read_setup( 'setups/sport.db.admin', WORLD_DB_INCLUDE_PATH, skip_tags: true )  # populate world tables
  # WorldDb.stats
end

task :importsport => :env do
  SportDb.read_builtin
  SportDb.read_setup( 'setups/all', SPORT_DB_INCLUDE_PATH )
  # SportDb.stats
end

task :deletesport => :env do
  SportDb.delete!
end


desc 'build sport.db from scratch'
task :build => [:clean, :create, :importworld, :importsport] do
  puts 'Done.'
end

desc 'update sport.db'
task :update => [:deletesport, :importsport] do
   puts 'Done.'
end

