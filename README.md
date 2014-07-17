This is a simple (working) Promise library for Apple's Swift language. Since it's becomming more like Rx than a Promise library, I'll be changing the naming conventions soon.

Futures and Promises can be hot (default) and cold. Cold streams cache their history and replay it to new handlers.

Examples:

    // A Promise contains two Observables to monitor sent data and raised errors
    var p = Promise<Int,Int>(isCold:false);
    
    p.send(2); // ignored since the Deffered is hot and no handles have been placed on the promise yet
    
    var mf = p.success
        .filter() { $0 < 100 } // filter items less than 100 from the stream
        .map() { "World " + String($0) } // change the stream from ints to strings
        .forEach() { (x) in println(x) }; // forEach does not change the stream output

    p.send(98);
    p <= 99; // shorthand for d.send(99)
    p <= 105; // filtered since 105 is not < 100

    //Merging
    var p2 = Promise<String,String>();
    p2.success.merge(mf).on() { // fires when both streams are fulfilled
       println( $0 ); // prints ("Hello", "World 1") of type Tuple<String, String>
    }
    p2 <= "Hello"; // fulfill promise in d2
    p <= 1; // update promise p
    p <= 100; // filtered because of the filter on mf
    
**See [main.swift](https://github.com/jadbox/ASwiftPromise/blob/master/ASwiftPromise/main.swift) for more examples!**

[@jonathanAdunlap](http://twitter.com/jonathanAdunlap)
