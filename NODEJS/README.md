### JavaScript Core

## var, let, const
    var → function scoped (old, avoid)
    let → block scoped (changeable)
    const → block scoped (not reassignable)
    Tip: Prefer const by default, use let only when needed.

##   Hoisting
        -    Hoisting is JavaScript’s behavior where variable and function declarations are moved to the top of their scope during compilation.
        -   var is hoisted and initialized with undefined, while let and const are hoisted but remain in a Temporal Dead Zone until initialized.
        -   Function declarations are fully hoisted, but function expressions are not.

        | Type                 | Hoisted | Initialized   | Behavior       |
        | -------------------- | ------- | ------------- | -------------- |
        | `var`                | ✅ Yes   | ✅ `undefined` | No error       |
        | `let`                | ✅ Yes   | ❌ No          | ReferenceError |
        | `const`              | ✅ Yes   | ❌ No          | ReferenceError |
        | Function Declaration | ✅ Yes   | ✅ Yes         | Works          |
        | Function Expression  | ❌ No    | ❌ No          | Error          |


##   Closures
        A closure is when a function remembers and can access variables from its outer (parent) scope, even after the outer function has finished executing.
        
        ## Code example
        function outer() {
        let count = 0;

        function inner() {
            count++;
            console.log(count);
        }

        return inner;
        }

        const fn = outer();
        fn(); // 1
        fn(); // 2
        fn(); // 3
        🔍 What’s happening?
        outer() runs → creates variable count
        It returns inner function
        Even after outer() is done…
        inner() STILL remembers count

        👉 That memory = closure

##   Prototype & prototype chain
        -   Every JavaScript object has a hidden property called [[Prototype]] (accessible via __proto__) that links to another object. This is called its prototype.

        ## Code example
        const parent = {
            greet: function () {
                console.log("Hello");
            }
        };

        const child = {
            name: "Ali"
        };

        child.__proto__ = parent;
        child.greet(); // Hello

        🔍 Explanation
        child doesn’t have greet
        JS looks into parent
        Finds it → executes

        👉 That lookup process = prototype chain

        ## Code example
        function User(name) {
        this.name = name;
        }

        User.prototype.sayHello = function () {
        console.log("Hello " + this.name);
        };

        const u1 = new User("Nazir");
        u1.sayHello(); // Hello Nazir


        | Term        | Meaning                          |
        | ----------- | -------------------------------- |
        | `prototype` | Property of constructor function |
        | `__proto__` | Property of object instance      |


        ❓ How does JS find a property?
        👉 Answer:

        JavaScript first looks for the property in the object itself. If not found, it traverses up the prototype chain until it finds the property or reaches null.

        Object Instance (u1)
                ↓
        User.prototype
                ↓
        Object.prototype
                ↓
        null

        ⚠️ Important Points

        ✔ Everything in JS is object (except primitives)
        ✔ Functions are also objects
        ✔ Prototype enables inheritance
        ✔ Chain lookup affects performance (deep chain = slower)

        Object → Parent → Grandparent → null

        🚀 Real Usage in Node.js

        Prototypes are used in:

        Express middleware (internally)
        EventEmitter
        Custom classes
        Framework internals

        🧠 Perfect Interview Answer

        If asked: What is prototype and prototype chain?

        In JavaScript, every object has an internal reference to another object called its prototype. When a property is accessed, JavaScript first checks the object itself, and if not found, it looks up the prototype chain until it finds the property or reaches null. This mechanism enables inheritance and method sharing.

##   this keyword

        ✅ What is this?
        👉 Definition:

        this refers to the object that is currently executing the function. Its value is determined by how a function is called, not where it is written.

        🧠 Golden Rule (Must Remember)
        "this depends on HOW a function is called"

        🟢 1️⃣ this in Global Scope
        console.log(this);
        👉 In browser: window
        👉 In Node.js: {}

        👉 Because Node uses modules (not global window)


        🟢 2️⃣ this inside a Regular Function
        function show() {
        console.log(this);
        }

        show();

        👉 In strict mode: undefined
        👉 In non-strict mode: global object
 
        🟢 3️⃣ this inside an Object Methods
        const user = {
            name: "Nazir",
            greet: function () {
                console.log(this.name);
            }
        };

        user.greet(); // Nazir

        👉 Here: this = user object

        🔴 Important Twist
        const user = {
            name: "Nazir",
            greet: function () {
                function inner() {
                console.log(this.name);
                }
                inner();
            }
        };

        user.greet(); // undefined
        👉 Why?

        inner() is a normal function
        Not called as object method

        ✅ Fix using Arrow Function
        const user = {
            name: "Nazir",
            greet: function () {
                const inner = () => {
                console.log(this.name);
                };
                inner();
            }
        };
        user.greet(); // Nazir

        👉 Arrow function inherits this from parent

        🟡 4️⃣ Arrow Function and this
        👉 Rule: Arrow functions DO NOT have their own this. They use parent scope this
        
        const obj = {
        name: "Ali",
        greet: () => {
            console.log(this.name);
        }
        };

        obj.greet(); // undefined
        👉 Because: Arrow function takes this from outside (global)

##   Arrow functions vs normal functions

    🧠 Interview Answer (Perfect)

    Arrow functions are a shorter syntax for writing functions in JavaScript. The key difference is that arrow functions do not have their own this; instead, they inherit this from the surrounding scope. They also do not have their own arguments, cannot be used as constructors, and do not have a prototype. Normal functions, on the other hand, have dynamic this, support constructors, and are more suitable for object methods and class-based logic.

##   Event loop basics
    The Event Loop is a mechanism that allows Node.js to handle multiple operations using a single thread.
    
    🧠 Simple Analogy (Very Powerful)

        Think of a restaurant waiter 🍽️:

        Takes order from customer
        Sends it to kitchen
        Doesn’t wait there ❌
        Goes to next customer ✅
        When food is ready → serves it

        👉 That waiter = Event Loop

    🧠 Key Idea
        Node.js is single-threaded BUT non-blocking 👉 It doesn’t wait → it delegates tasks


    🔧 Core Components
        1. Call Stack
            Executes synchronous code
            Works like a stack (LIFO)

        2. Web APIs / Node APIs
            Handles async tasks (setTimeout, file, DB)
        
        3. Callback Queue
            Stores completed async callbacks
        
        4. Event Loop
            Moves callbacks to call stack when ready

    🔄 Flow Diagram
        Call Stack → Web API → Callback Queue → Event Loop → Call Stack

    Code Example
    console.log("Start");

    setTimeout(() => {
    console.log("Timeout");
    }, 0);

    console.log("End");

    ⚡ Microtask vs Macrotask

    | Type      | Examples                  | Priority |
    | --------- | ------------------------- | -------- |
    | Microtask | Promise, process.nextTick | 🔥 High  |
    | Macrotask | setTimeout, setImmediate  | Lower    |

    Execution Order:
    1. Sync code
    2. Microtasks (Promises)
    3. Macrotasks (setTimeout)


    🧠 Interview Answer (Perfect)

        The event loop is a mechanism in Node.js that allows it to handle asynchronous operations using a single thread. It continuously checks the call stack and callback queue, and when the stack is empty, it pushes queued callbacks to the stack for execution. It enables non-blocking I/O by delegating operations to system APIs and processing their callbacks later.


    🔥 Visual Summary
        Call Stack (sync)
            ↓
        Web APIs (async)
            ↓
        Callback Queue
            ↓
        Event Loop
            ↓
        Back to Call Stack


    🔥 1️⃣ Event Loop Phases (Node.js Internals)

    Node.js Event Loop works in phases (steps):
    1. Timers
    2. Pending Callbacks
    3. Idle / Prepare
    4. Poll  ← (MOST IMPORTANT)
    5. Check
    6. Close Callbacks


    🧠 Easy Analogy

    Think of a factory assembly line 🏭:

    Each phase = one station
    Tasks move step-by-step
    Some tasks get priority

    🔍 Most Important Phases

    🔹 1. Timers Phase
        Runs:
        setTimeout
        setInterval

    setTimeout(() => console.log("Timer"), 0);
    👉 Executes when timer expires (not immediately)

    🔹 2. Poll Phase (🔥 Core of Node)
        👉 This is where:
        Incoming requests are handled
        I/O callbacks run (DB, file, API)

        👉 If nothing to do:
        Event loop waits here

    🔹 3. Check Phase
        Runs:
        setImmediate()

    ⚡ Important Order
    Timers → Poll → Check

    🔥 2️⃣ process.nextTick vs Promise vs setTimeout vs setImmediate

    This is a VERY COMMON senior interview question.

    🧠 Priority Order (REMEMBER THIS)
    1. process.nextTick()
    2. Promise (microtask)
    3. setTimeout / setImmediate (macrotask)
    
    💻 Example (IMPORTANT)
    console.log("1");

    setTimeout(() => console.log("2"), 0);

    setImmediate(() => console.log("3"));

    Promise.resolve().then(() => console.log("4"));

    process.nextTick(() => console.log("5"));

    console.log("6");

    🔍 Output
    1
    6
    5
    4
    2 or 3 (depends)

    🔥 3️⃣ Microtask vs Macrotask (Deep Understanding)

        🔹 Microtasks
            process.nextTick
            Promise.then

        👉 Executed: Immediately after current execution

        🔹 Macrotasks
            setTimeout
            setImmediate
            I/O

        👉 Executed in event loop phases

        🧠 Execution Flow
        1. Run sync code
        2. Run all nextTick queue
        3. Run all Promise queue
        4. Run event loop phases (timers, poll, check)

    🔥 4️⃣ Real Interview Insight

        ❓ Why process.nextTick is dangerous?

        function loop() {
            process.nextTick(loop);
        }
        loop();

        👉 This will:

        ❌ Block event loop
        ❌ Starve I/O
        ❌ Crash app

    🔥 5️⃣ Senior-Level Explanation (Use in Interview)

        In Node.js, the event loop processes tasks in phases like timers, poll, and check. Additionally, there are microtask queues like process.nextTick and Promises that execute before the next phase begins. process.nextTick has the highest priority and runs before Promises, while setTimeout and setImmediate are executed in different phases of the event loop. The execution order can vary depending on context, especially between setTimeout and setImmediate.

        🧠 Final Memory Trick (SUPER IMPORTANT)

        Sync
        ↓
        nextTick 🔥
        ↓
        Promise 🔥
        ↓
        Timers (setTimeout)
        ↓
        Poll (I/O)
        ↓
        Check (setImmediate)

##   Callbacks
##   Promises
##   Async / Await
##   Error handling (try/catch)

