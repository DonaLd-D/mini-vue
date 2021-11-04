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

