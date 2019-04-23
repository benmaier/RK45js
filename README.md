# RK45js

This is a browser-version of https://github.com/imiRemy/RK45js (adaptive step-size Runge-Kutta 4(5) solver).

## Install

Use in html as

```html
<script src="./lib/rk45.js"></script>
```

or

```html
<script src="./dist/rk45-min.js"></script>
```

## Examples

### Single integration

```js
var diffEqX0 = function( time, x ) { return x[0]; }
var initCoord = [ 1 ];

var foo = new rk45();

foo.setStart( 0.0 );        // Initial start time, t=0.
foo.setStop( 1.0 );         // Time at which we want a solution, t=1.
foo.setInitX( initCoord.slice() );  // y(0) -- value of y when t=0.
foo.setFn( [diffEqX0] );    // Differential equation we're solving.

foo.solve();

console.log("result: " + foo.newX);
```

### Integrate array of times

```js
var diffEqX0 = function( time, x ) { return x[0]; }
var initCoord = [ 1 ];

var bar = new rk45();
bar.setInitX( initCoord.slice() );
bar.setFn( [diffEqX0] );    // Differential equation we're solving.

var times = [0,1,2,3,4];
var y = bar.solveTimes(times);
for(let i=0; i<times.length; ++i)
    console.log("t =", times[i], "computed = "+ y[i], "exact =", Math.exp(times[i]));
```

### Iteratively update new times and integrate

```js
var diffEqX0 = function( time, x ) { return x[0]; }
var initCoord = [ 1 ];

var foobar = new rk45();
foobar.setInitX( initCoord.slice() );
foobar.setFn( [diffEqX0] );    // Differential equation we're solving.
foobar.setStart( 0.0 );        // Initial start time, t=0.

var t = 0;
var tmax = 4;
var dt = 1;
while (t < tmax)
{
    // advance time
    t = t+dt;
    foobar.setStop(t);

    // solve up to this time
    foobar.solve();

    // return result
    console.log("t =", t, "y =", foobar.newX.slice());

    // tell the integrator to adopt the current state as new initial state
    foobar.adoptCurrentState();
}
```

## More info from original repository

JavaScript Implementation of Runge-Kutta-Fehlberg Numerical Integration

The Runge-Kutta-Fehlberg method (RK45 method) is a numerical integration routine for solving systems of differential equations.  RK45 differs from the normal Runge-Kutta algorithm by featuring an *adapative* step size. As the integration proceeds, if the step size is too big (i.e. produces an error larger than a set value) then the step size is decreased until the error is within an acceptable value.  Conversely, if the error is much smaller than the tolerance, the step size is increased to improve computational efficiency.

This implementation is written in JavaScript and is based loosely off an older C version I wrote some time ago.

### Features:
- Computational solver can be instantiated as an object.
- No fixed limit to the order (size) of the system that can be solved.
- Sanity checking built-in prior to starting computation.
- Unit test suite (Mocha + Chai) for testing.

### Getting Started:
At the most basic level, you need to do the following:
- Define the differential equations(s) you are solving
- Instantiate a solver
- Set the intial conditions
- Specify the start and stop values in the independent variable; e.g. time.
- Call the solver.
- Read out your answer!

See the "rk45_sample.html" file for an example of getting started with a one degree of freedom (first order) system.

### License

MIT
