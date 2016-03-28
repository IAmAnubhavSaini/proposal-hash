A proposal for ES Next.

# proposal-hash
Takes (multiple) objects and return all the properties that are owned by the objects and not in prototype chain.


What is it?
-

```JS

var obj = #someObj;

```

1. `obj` will be a new **object**; not an array like what `Object.[keys|values|enteries]` return.
2. `obj` will contain only those properties that are `someObj`'s own.
3. Basically,

```JS

  var obj = {};
  Object.assign(obj, someObj);
  
```

### Why isn't Object.assign enough then?

Object.assign looks lengthier and unclean.

```JS

	var obj = {};
	for ( vax x in Object.assign(obj, someObj) ) {
		console.log(x, obj[x]);
	}
	
```

Contrast it with:

A sharp looking example:

```JS

	var obj = #someObj;
	for ( var x in obj ) {
		console.log(x, obj[x]);
	}

```

or even better:

A sharper, condensed version

```JS

	for ( var x in #someObj ) {
		console.log(x, someObj[x]);
	}

```

Checkout the `#someObj` part in code above. It looks so simple and intuitive.

### But Object.assign can assign properties from multiple objects.

```JS

	var x = {};
	Object.assign(x, A, B, C);

```

The code above will make `x` contain all the own properties of objects A, B and C.

> **Well, so can hash operator:**

```JS

	var x = {};
	x = A # B # C;

```

Or even better, in case where your object is not even initialized, `#` will initialize and treat object to be empty and then go ahead to fill it with the appropriate properties.

```JS

	x #= A # B # C;

```


### So it's just syntactic sugar?

You can say that. But tell me will you feel awesome if `#` worked with arrays too?

```JS

	Array.prototype.greeting = "Hello, World!";
	// Okay, anyone in their right mind won't do it, but whatever.
	var arr = [1, 2, 3];
	for ( var x in #arr ) {
		console.log(x, arr[x]);
	}

```

So, Arrays too can have properties that are not their own. This way, old constructs like `for..in` loop can work against returned value and stay clear of prototypical mess.

### but we already have `{ ...objA, ...objB }`

That looks like syntactic sugar too. But is it really as simple as `objA # objB`?
 
### What else can it do?

> Make objects lightweight

It can remove prototype chain or inheritance in one fell swoop.

```JS

	x = #x;

```



Why hash?
-

Because:

1. `hasOwnProperty` starts with `has`
2. `#` is also known as `hash`

### No. Why use a new operator?

Because it makes language a bit less verbose. Think about ` ? : ` operator. We can achieve:

```JS

	var out = null;
	if ( x ) {
		out = A; // anything really, x.A
	} else {
		out = B; // anything really, default
	}

```

But we go for:

```JS

	var out = x ? A : B ;

```

Because it makes sense and is less verbose.

> And `#` wasn't in use anyway.


Summary
-

1. `#someObj` returns all the properties that satisfy `hasOwnProperty` in a new object.
2. It works with arrays, objects, iterables, dictionaries, anything that is a  key-value store, even yielding functions.
3. `#` doesn't evaluate properties of the objects.
4. `#` just gets the references or shallow-copies in key-value format from objects.
5. `#` removes prototype-chain and inheritance if used like: ` x = #x; `

