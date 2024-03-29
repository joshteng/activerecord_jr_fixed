require 'faker'

task :console do
  exec "irb -r./app.rb"
end

namespace :db do
  task :seed do
    require_relative 'app'
    cohort_names = %w(Alpha Beta Delta Gamma Epsilon Zeta Eta Theta Iota)

    cohort_ids = cohort_names.map do |name|
      Cohort.create(:name => name)[:id]
    end

    2000.times do
      Student.create :first_name => Faker::Name.first_name,
                     :last_name  => Faker::Name.last_name,
                     :email      => Faker::Internet.email,
                     :birthdate  => Date.today - rand(15..40)*365,
                     :gender     => ['m', 'f'].sample,
                     :cohort_id  => cohort_ids.sample
    end
  end

  task :setup do
    require_relative 'app.rb'

    print "Creating database at #{Database::Model.filename}..."

    Database::Model.execute(<<-SQL)
      CREATE TABLE "students" (
        "id" INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
        "cohort_id" integer,
        "first_name" varchar(255),
        "last_name" varchar(255),
        "email" varchar(255),
        "gender" varchar(255),
        "birthdate" date,
        "created_at" datetime NOT NULL,
        "updated_at" datetime NOT NULL
      );
    SQL

    Database::Model.execute(<<-SQL)
      CREATE TABLE "cohorts" (
        "id" INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL,
        "name" varchar(255),
        "created_at" datetime NOT NULL,
        "updated_at" datetime NOT NULL
      );
    SQL

    puts "done"
  end
end


namespace :test do
  task :run do
    require_relative 'app.rb'

    cohort = Cohort.find(1)

    puts cohort[:name] == 'Alpha'
    cohort[:name] = 'Best Cohort Ever'
    cohort.save
    puts Cohort.find(1)[:name] == 'Best Cohort Ever'
    cohort[:name] = 'Alpha'
    cohort.save

    puts Cohort.all.first[:name] == 'Alpha'

    student = Student.create(:first_name => "Chris",
                     :last_name  => Faker::Name.last_name,
                     :email      => Faker::Internet.email,
                     :birthdate  => Date.today - rand(15..40)*365,
                     :gender     => 'm',
                     :cohort_id  => 1
                     )
    
    student = Student.find(student[:id])
    puts student[:first_name] == "Chris"

    cohort = Cohort.where('id = ?', 1).first
    puts cohort[:name] == "Alpha"

    # def new_record?
    #   self[:id].nil?
    # end

    new_cohort = Cohort.new

    puts new_cohort[:id] == nil


  end
end





