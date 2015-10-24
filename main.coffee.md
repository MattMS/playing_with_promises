# Promise test

- [Promise @ MDN](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise)

	get_promise = (passes, value)->
		new Promise (resolve, reject)->
			if passes
				resolve value
			else
				reject value

	test_then_then_catch = (pass1, pass2, after)->
		get_promise pass1, 1
		.then (value)->
			console.log "(#{pass1}, #{pass2}) [then]-then-catch "
			get_promise pass2, value
		.then (value)->
			console.log "(#{pass1}, #{pass2}) then-[then]-catch"
			after()
		.catch (value)->
			console.log "(#{pass1}, #{pass2}) then-then-[catch]"
			after()

	test_then_catch_then = (pass1, pass2, after)->
		get_promise pass1, 1
		.then (value)->
			console.log "(#{pass1}, #{pass2}) [then]-catch-then"
			get_promise pass2, value
		.catch (value)->
			console.log "(#{pass1}, #{pass2}) then-[catch]-then"
			after()
		.then (value)->
			console.log "(#{pass1}, #{pass2}) then-catch-[then]"
			after()

	test_catch_then_then = (pass1, pass2, after)->
		get_promise pass1, 1
		.catch (value)->
			console.log "(#{pass1}, #{pass2}) [catch]-then-then"
			after()
		.then (value)->
			console.log "(#{pass1}, #{pass2}) catch-[then]-then"
			get_promise pass2, value
		.then (value)->
			console.log "(#{pass1}, #{pass2}) catch-then-[then]"
			after()

	pairs = [
		[1, 1]
		[1, 0]
		[0, 1]
		[0, 0]
	]

	test_pairs = (tester)->
		test_next = (index)->
			if index >= 0
				[pass1, pass2] = pairs[index]
				tester pass1, pass2, ->
					test_next index - 1

		test_next pairs.length - 1

		#for pair in pairs
			#tester pair...

	#test_pairs test_then_then_catch
	test_pairs test_then_catch_then
	#test_pairs test_catch_then_then
