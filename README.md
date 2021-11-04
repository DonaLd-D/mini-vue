# defineproperty实现数据响应式

```
//要被转换成响应式的数据
let data={
    a:1,
    b:2
}
//vm模拟vue实例，data的数据转换成响应式的同时，挂载在vue的实例上
let vm={}

Object.keys(data).forEach(key=>{
    Object.defineProperty(vm,key,{
        enumerable:true,
        configurable:true,
        get(){
            return data[key]
        },
        set(newvalue){
            if(data[key]==newvalue) return
            data[key]=newvalue
        }
    })
})

vm.a=2
console.log(data.a) //2
```

# Proxy实现数据响应式
```
let data={
    a:1,
    b:2
}

let vm=new Proxy(data,{
    //target指向data
    get(target,key){
        return target[key]
    },
    set(target,key,newValue){
        if(target[key]==newValue) return
        target[key]=newValue
    }
})

vm.a=2
console.log(data.a) //2
```

# vue类
```
class Vue{
    constructor(options){
        this.$options=options?options:{}
        this.$el=typeof options.el=='string'?document.querySelector(options.el):options.el
        this.$data=options.data?options.data:{}
        this.proxyData(this.$data)
    }
    proxyData(data){
        Object.keys(data).forEach(key=>{
            Object.defineProperty(this,key,{
                enumerable:true,
                configurable:true,
                get(){
                    return data[key]
                },
                set(newValue){
                    if(data[key]==newValue) return
                    data[key]=newValue
                }
            })
        })
    }
}
```

# observer类
```
class Observer{
    constructor(data){
        this.walk(data)
    }
    walk(data){
        if(!data||typeof data!='object') return
        Object.keys(data).forEach(key=>{
            this.defineReactive(data,key)
        })
    }
    defineReactive(obj,key){
        //如果该key的值是对象，也处理为响应式的
        this.walk(obj[key])
        let self=this
        Object.defineProperty(obj,key,{
            enumerable:true,
            configurable:true,
            get(){
                return obj[key]
            },
            set(newValue){
                if(obj[key]==newValue) return
                obj[key]=newValue
                如果赋的新值是对象，也处理成响应式的
                self.walk(obj[key])
            }
        })
    }
}
```
```
class Vue{
    constructor(options) {
        //...
        new Observer(this.$data) //把vue实例的数据处理成响应式并被template使用
    }
    //把数据处理成响应式并挂载在vue实例
    proxyData(data){

    }
}
```
