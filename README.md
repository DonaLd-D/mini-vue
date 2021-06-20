# object.defineproperty和proxy的区别

```
//Object.defineProperty实现数据响应式

let vm={
    id:"juejin",
    name:"掘金"
}
let keys=Object.keys(vm)
keys.forEach(key=>{
    let value=vm[key]
    Object.defineProperty(vm,key,{
        get(){
            console.log("执行get")
            return value
        },
        set(newValue){
            console.log("执行set")
            if(newValue!=value){
                value=newValue
            }
        }
    })
})
vm.id="csdn"
console.log(vm,vm.id,vm.name)

//proxy实现数据响应式
let _vm={
    id:'juejin',
    name:'掘金'
}
let newvm=new Proxy(_vm,{
    get(target,key){
        console.log('执行get')
        return target[key]
    },
    set(targrt,key,newValue){
        console.log('执行set')
        if(targrt[key]!=newValue){
            targrt[key]=newValue
        }
    }
})
newvm.use='codercao'
newvm.id='csdn'
console.log(newvm)
```
