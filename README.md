
# Croalogix SDK - Ruby Implementation
This is an implementation of Coralogix Ruby SDK.

## Talbe of contents

1. Install
2. General
3. Ruby
4. Ruby on Rails
5. Ruby on Rails + Puma
6. Ruby on Rails + Unicorn


## Install

gem install coralogix_logger


## General

**Private Key** - A unique ID which represents your company, this Id will be sent to your mail once you register to Coralogix.

**Application Name** - The name of your main application, for example, a company named “SuperData” would probably insert the “SuperData” string parameter or if they want to debug their test environment they might insert the  “SuperData– Test”.

**SubSystem Name** - Your application probably has multiple subsystems, for example: Backend servers, Middleware, Frontend servers etc. in order to help you examine the data you need, inserting the subsystem parameter is vital.



## Ruby

You must provide the following four variables when creating a Coralogix logger instance.

**Private Key** - A unique ID which represents your company, this Id will be sent to your mail once you register to Coralogix.

**Application Name** - The name of your main application, for example, a company named “SuperData” would probably insert the “SuperData” string parameter or if they want to debug their test environment they might insert the  “SuperData– Test”.

**SubSystem Name** - Your application probably has multiple subsystems, for example: Backend servers, Middleware, Frontend servers etc. in order to help you examine the data you need, inserting the subsystem parameter is vital.

##### Example: Ruby usage ####
    require 'coralogix_logger'

    PRIVATE_KEY = "11111111-1111-1111-1111-111111111111"    
    APP_NAME = "Ruby Tester"  
    SUB_SYSTEM = "Ruby tester client"     

    # Configure Coralogix SDK. You need to define it only once per process.
    Coralogix::CoralogixLogger.configure(PRIVATE_KEY, APP_NAME, SUB_SYSTEM)

    # The common practice is to get an instance of the logger in each class and setting the logger name to the class name.
    # logger name will be used as category unless specified otherwise.
    logger = Coralogix::CoralogixLogger.get_logger("my class")

    # Send "Hello World!" message with severity verbose. 
    logger.log(Coralogix::Severity::VERBOSE, "Hello World!")

    # Additional options
    # Severity and message parameters are mandatory. The rest of the parameters are optional.
    logger.log(Coralogix::Severity::DEBUG, "Hello World!", category: "my category")
    logger.log(Coralogix::Severity::INFO, "Hello World!", category: "my category", className: "my class")
    logger.log(Coralogix::Severity::WARNING, "Hello World!", category: "my category", className: "my class", methodName: "my method")
    logger.log(Coralogix::Severity::ERROR, "Hello World!", category: "my category", className: "my class", methodName: "my method", threadId: "thread id")
    logger.log(Coralogix::Severity::CRITICAL, "Hello World!", className: "my class", methodName: "my method", threadId: "thread id")


    # Using severity methods
    # Only message is mandatory. The rest of the parameters are optional.
    logger.debug("Hello World!")
    logger.verbose("Hello World!", className: "my class")
    logger.info("Hello World!", className: "my class", methodName: "my method")
    logger.warning("Hello World!", className: "my class", methodName: "my method", threadId="thread id")
    logger.error("Hello World!", className: "my class", methodName: "my method", threadId="thread id")
    logger.critical("Hello World!", category: "my category", className: "my class", methodName: "my method", threadId="thread id")


## Ruby on Rails

1. Currently the coralogix SDK supports Rails version >= 4
2. Add the coralogix gem in the Gemfile:

##### Example: Gemfile ####
    gem 'coralogix_logger'

3. Run: bundle install
4. Create coralogix.rb file under: config/initializers/ folder.
5. Copy the following content into the file. Replace the constants with your values:

##### Example: Rails configuration ####
    require 'coralogix_logger'

    PRIVATE_KEY = "11111111-1111-1111-1111-111111111111"
    APP_NAME = "Ruby Rails tester"
    SUB_SYSTEM = "Ruby Rails tester client"

    Coralogix::CoralogixLogger.configure(PRIVATE_KEY, APP_NAME, SUB_SYSTEM)
    coralogix_logger = Coralogix::CoralogixLogger.get_logger(SUB_SYSTEM)
    Rails.logger.extend(ActiveSupport::Logger.broadcast(coralogix_logger))

6. Now you can use your Rails logger as usual:

##### Example: Rails usage ####
    Rails.logger.info "Hello World from Rails!"


## Ruby on Rails + Puma

1. Configure Rails logger as explained above in the documentation.
2. Copy the following content in the puma config file: config/puma.rb

##### Example: Puma configuration ####

    require 'coralogix_logger'

    on_worker_boot do
        Coralogix::CoralogixLogger.reconnect
    end



## Ruby on Rails + Unicorn

1. Configure Rails logger as explained above in the documentation.
2. Copy the following content in the unicorn config file: config/unicorn.rb

##### Example: Unicorn configuration ####

    require 'coralogix_logger'

    after_fork do |server, worker|
        Coralogix::CoralogixLogger.reconnect
    end
