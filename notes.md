Exception Handling
==================

Don't handle them. Make it so you won't need to deal with them.

Risky code:

	[1,2,3,nil].each do |number| 
		puts number + 1
	end

This code is more robust:
- compacting gets rid of array
- to_i makes sure you're adding to an integer

	[1,2,3,nil].compact do |number|
		puts number.to_i + i
	end

Handling the exceptions:

	begin
		[1,2,3,nil].each do |number|
			puts number.to_i + i
		end
	rescue NoMethodError => exception
		warn "Error: Something bad happened: #{exception}"
	end
	
Can chain exception rescuing like this:

	begin
		...
	rescue Exception1, Exception2 => exception
		...
	end
	
Something that you want to always happen:

	begin
		...
	rescue Exception1 => exception
		...
	ensure
		# will always happen
	end

Can also have an else for rescues:
	def method_name
		begin
			...
		rescue Exception1 => exception
			...
		else
			# Runs if there were no exceptions.
		ensure
			...
		end
	end

Alternative way to handle method wide exceptions, don't use the begin, instead hang the recuse, else, ensure, end off the method:

	def method_name
		variable = something
		...
	rescue Exception1 => exception
		...
	else
		# Runs if there were no exceptions.
	ensure
		...
		variable #still accessible down here
	end
	
If one or more methods are handling similar exceptions around different functionality, can make more generic error handling using a block:

	# Call this with a block.
	def safe_connection
		RemoteConnection.open!
		yield if block_given?
	rescue Exception => exception
		warn "blah"
	ensure
		RemoteConnection.close!
	end

	safe_connection do
		RemoteConnection.info :username => "Frank"
	end
	#or
	safe_connection { RemoveConnection.info :username => "Frank" }
	

Handy link: [Closure explanations](http://www.skorks.com/2010/05/closures-a-simple-explanation-using-ruby/)

Book: Exceptional Ruby by Avdi Grimm (PDF)