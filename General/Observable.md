# Observable

Basic Observable Patterm;

## JavaScript Example

```javascript

// First solution (With class)
class Observable {
  constructor(value){
    this._value = value;
    this._listeners = [];
  }

  get value(){
    return this._value;
  }

  set value(newValue){
    if (this._value !== newValue) {
      this._value = newValue;
      this._notify(newValue);
    }
  }

  subscribe(listener){

    this._listeners.push(listener);
    const listenerIndex = this._listeners.length - 1;

    return () => {
      this._listeners.splice(listenerIndex, 1)
    }
  }

  _notify(value){
      this._listeners.forEach((listener) => listener(value))
  }
}

// Class but with proxy
class Observable {
  constructor(target) {
    this.target = target;
    this.listeners = [];
    
    return this._makeProxy(target);
  }

  _makeProxy(target) {
    const self = this;

    return new Proxy(target, {
      get(obj, prop) {
        return obj[prop];
      },

      set(obj, prop, value) {
        if (obj[prop] !== value) {
          const oldValue = obj[prop];
          obj[prop] = value;
          self._notify(prop, oldValue, value);
        }
        return true;
      }
    });
  }

  _notify(prop, oldValue, newValue) {
    this.listeners.forEach(listener => {
      listener(prop, oldValue, newValue);
    });
  }

  watch(listener) {
    this.listeners.push(listener);
  }
}

const data = new Observable({
  count: 0,
});

data.watch((property, oldValue, newValue) => {
  console.log(`Property ${property} changed from ${oldValue} to ${newValue}`);
});

data.count = 1; // Property count changed from 0 to 1
data.count = 2; // Property count changed from 1 to 2



// Kinda vue3 like reactive
function reactive(obj) {
  const listeners = {};

  return new Proxy(obj, {
    get(target, prop) {
      return target[prop];
    },

    set(target, prop, value) {
      if (target[prop] !== value) {
        target[prop] = value;
        if (listeners[prop]) {
          listeners[prop].forEach(fn => fn(value));
        }
      }
      return true;
    }
  });
}

function watch(obj, key, fn) {
  if (!obj._listeners[key]) {
    obj._listeners[key] = [];
  }
  obj._listeners[key].push(fn);
}

const data = reactive({
  count: 0,
  _listeners: {}
});

watch(data, 'count', (newValue) => {
  console.log(`New count value: ${newValue}`);
});

data.count = 1; // New count value: 1
data.count = 2; // New count value: 2


//Kinda vue3 again
const reactiveHandlers = {
  get(target, prop, receiver) {
    const value = Reflect.get(target, prop, receiver);
    if (typeof value === 'object' && value !== null) {
      return reactive(value);
    }
    return value;
  },

  set(target, prop, value, receiver) {
    const oldValue = Reflect.get(target, prop, receiver);
    const result = Reflect.set(target, prop, value, receiver);
    if (oldValue !== value) {
      trigger(target, prop);
    }
    return result;
  }
};

function reactive(target) {
  if (typeof target !== 'object' || target === null) {
    return target;
  }
  return new Proxy(target, reactiveHandlers);
}

const effectStack = [];
function effect(fn) {
  const run = () => {
    effectStack.push(run);
    fn();
    effectStack.pop();
  };
  run();
}

const targetsMap = new WeakMap();

function track(target, key) {
  let depsMap = targetsMap.get(target);
  if (!depsMap) {
    targetsMap.set(target, (depsMap = new Map()));
  }
  let dep = depsMap.get(key);
  if (!dep) {
    depsMap.set(key, (dep = new Set()));
  }
  if (effectStack.length > 0) {
    dep.add(effectStack[effectStack.length - 1]);
  }
}

function trigger(target, key) {
  const depsMap = targetsMap.get(target);
  if (!depsMap) return;
  const dep = depsMap.get(key);
  if (dep) {
    dep.forEach(effect => effect());
  }
}

const data = reactive({
  count: 0,
  info: {
    name: 'Alice'
  }
});

effect(() => {
  console.log(`Count value: ${data.count}`);
});

effect(() => {
  console.log(`Name value: ${data.info.name}`);
});

data.count = 1; // Count value: 1
data.info.name = 'Bob'; // Name value: Bob


