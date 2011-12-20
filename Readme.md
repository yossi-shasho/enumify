# Enumify [![Build Status](https://secure.travis-ci.org/yonbergman/enumify.png)](http://travis-ci.org/yonbergman/enumify)


enumify adds an enum command to all ActiveRecord models which enables you to work with string attributes as if they were enums

## Installing

Just add the enumify gem to your GemFile

## How to use

Just call the enum function in any ActiveRecord object, the function accepts the field name as the first variable and the possible values as an array

    class Event < ActiveRecord::Base
        enum :status, [:available, :canceled, :completed]
    end

After that you get several autogenerated commands to use with the enum

    # Access through field name

    event.status                # returns the enum's current value as a symbol
    event.status = :canceled    # sets the enum's value to canceled (can also get a string)


    # Shorthand methods, access through the possible values

    event.available?            # returns true if enum's current status is available
    event.canceled!             # changes the enum's value to canceled

    # Get all the possible values
    
    Event::STATUSES             # returns all available status of the enum

## Callbacks
Another cool feature of enumify is the option to add a callback function that will be called each time the value of the field changes
This is cool to do stuff like log stuff or create behaviour on state changes

All you need to do is add a x_changed method in your class and the enumify will call it

    class Event < ActiveRecord::Base
        enum :status, [:available, :canceled, :completed]

        def status_changed(old, new)
            puts "status changed from #{old} to #{new}"
        end
    end


## Scopes
One last thing that the enumify gem does is created scope (formerly nested_scopes) so you can easly query by the enum

For example if you want to count all the events that are canceled you can just run

    Event.canceled.count


---

Copyright (c) 2011 Yonatan Bergman, released under the MIT license