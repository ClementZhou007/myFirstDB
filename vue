<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="./lib/vue.js"></script>
    <link rel="stylesheet" href="./lib/bootstrap-3.3.7-dist/css/bootstrap.min.css">
</head>
<body>
   <div id="app">
    
    
    <div class="panel panel-primary">
          <div class="panel-heading">
                <h3 class="panel-title">添加品牌</h3>
          </div>
          <div class="panel-body form-inline">
                <label >
                    ID:
                    <input type="text" class="form-control" v-model="id">
                </label>

                <label >
                    Name:
                    <input type="text" class="form-control" v-model="name">
                </label>

                <input type="button" value="添加" class="btn btn-primary" 
                  @click="add">
                  <label >
                    搜索名称关键字：
                    <input type="text" class="form-control" v-model="keywords">
                  </label>
          </div>
    </div>
    

    <table class="table table-bordered table-hover table-striped">
       <thead>
           <tr>
               <th>ID</th>
               <th>Name</th>
               <th>Ctime</th>
               <th>Operation</th>
           </tr>
       </thead>
       <tbody>
           <tr v-for="item in search(keywords)" :key="item.id">
               <td>{{ item.id }}</td>
               <td v-text="item.name"></td>
               <td>{{ item.ctime | dateFormat  }}</td>
               <td><a href="" @click.prevent="del(item.id)">删除</a></td>
           </tr>
       </tbody>
   </table>
</div>

<script>
    //定义全局过滤器
    Vue.filter('dateFormat',function(dateStr,pattern=""){
        //根据给定的时间字符串，得到特定的时间
        var dt=new Date(dateStr)

        //yyyy-mm-dd
        var y=dt.getFullYear()
        var m=dt.getMonth()+1
        var d=dt.getDate()
       
        // return y+'-'+ m + '-' +d
        if(pattern.toLowerCase()=='yyyy-mm-dd'){
            return `${y}-${m}-${d} `
        }else{
            var hh=dt.getHours()
            var mm=dt.getMinutes()
            var ss=dt.getSeconds()
            return `${y}-${m}-${d}  ${hh}:${mm}:${ss}`
        }
    })


    //创建Vue实例，得到ViewModel
    var vm=new Vue({
        el:'#app',
        data:{
            id:'',
            name:'',
            keywords:'',
            list:[
                {id : 1, name: '奔驰', ctime : new Date()},
                {id : 2, name: '宝马', ctime : new Date()},
                {id : 3, name: '凯迪拉克', ctime : new Date()},
                {id : 4, name: '雷克萨斯', ctime : new Date()}
                ]
        },
        methods:{
            add(){
                var car={ id: this.id, name : this.name, ctime : new Date()}
                this.list.push(car)
                this.id= this.name=''
            },
            del(id){
   
                //查找索引 并删除
               var index= this.list.findIndex(item =>{
                    if(item.id==id){
                        return true;
                    }
                    
                })
                this.list.splice(index,1)
            },
            search(keywords){
                // var newList=[]
                // this.list.forEach(item => {
                //     if(item.name.indexOf(keywords) !=-1){
                //         newList.push(item)
                //     }
                // });
                // return newList

                //数组循环 ：forEach  some filter findIndex
                return  this.list.filter(item=>{
                    //ES6中为字符串提供了一个新方法，
                    //String.prototype.includes('要包含的字符串')
                    //包含则返回true
                    if(item.name.includes(keywords)){
                         return item
                    }
                })
            }
        }
    });
</script>
   
</body>
</html>
