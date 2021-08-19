

<div class="container mt-5">
    
<div id="list" class="list_container">
    
    <div class="models">
        <!--view item model-->
        <div class="item_model show-{viewItem_model}">
            <div class="header d-flex justify-content-between mb-2">
                <span></span>
                <div on:click="{()=>viewItem_model = false}" class="bg-danger rounded-circle close d-flex justify-content-center align-items-center"><img src="/x-circle.svg" alt="close icon"></div>
            </div>
            <h5>{itemInView_data.title}</h5>
            <div>{itemInView_data.comment}</div>
        </div>
        <!--/view item model-->

 
        <!--add item model-->
        <div class="item_model show-{addListItem_show}">
            <div class="header d-flex justify-content-between mb-2">
                <span></span>
                <div on:click="{addListItem_model_show}" class="bg-danger rounded-circle close d-flex justify-content-center align-items-center"><img src="/x-circle.svg" alt="close icon"></div>
            </div>
            <form action="#" class="d-flex flex-wrap align-items-center flex-column">
                <input id="listTitle" type="text" bind:this="{newItemInput}" bind:value="{newItemInputcontent}"  placeholder="Enter Title" class="input me-1 mb-2">
                <textarea class="newItemCommentInput bg-light" bind:this="{newItemCommentInput}" placeholder="Any Comments"></textarea>
                <button on:click|preventDefault ="{createItem}" class="btn btn-light mt-3  mt-sm-0">Add</button>
            </form>
        </div>
        <!--add item /model-->
    </div>


    <div class="list_container-header">
        <div class="container d-flex flex-wrap justify-content-between">
            <h2>Todo List</h2>
            <button on:click="{addListItem_model_show}" class="btn new-item-btn">
                Add Item
            </button>
        </div>
    </div>
    <div class="list-items mt-3" id="listItems">
        <div class="container">
            <ul>
                {#each list_items as item }
                    <li class="list-item done-{item.done} mb-3">
                        <div class="wrap d-flex flex-wrap align-items-center justify-content-between">
                            <span class="title d-flex align-items-center flex-wrap done-{item.done} ">
                                <div on:click="{checkOut(item)}" class="item-bullet me-2 done-{item.done}"></div>
                                <span on:click="{viewItem(item)}">{item.title}</span>
                            </span>
                            <div class="menu">
                                <button class="btn btn-light" title="Remove list item"><img src="/trash.svg" alt="trash icon" on:click="{removeItem(item)}"></button>
                            </div>
                        </div>
                    </li>
                {/each}
            </ul>
        </div>
    </div>
</div>

</div>

<script>

    import _ from 'lodash'
    import cookie from 'js-cookie'
    //cookie.remove('list')


    let list_items 

    if(cookie.get('list')=== undefined){

        list_items = [{
            title:"Start using Listiflyer ✨",
            comment:"",
            done:true
        }]

        cookie.set('list', JSON.stringify(list_items))

        console.log('list initialized');
        console.log(cookie.get('list'));
    }else{
        list_items = JSON.parse(cookie.get('list'))
        console.log(list_items);
    }


    function save(){
        cookie.set('list', JSON.stringify(list_items))
        list_items = JSON.parse(cookie.get('list'))
    }


    function checkOut(item){
        if (item.done === false){
            item.done = true
            list_items = list_items
        }else{
            item.done = false
        }
        save()
        
    }


    let viewItem_model = false

    let itemInView_data = {
        title:String,
        comment:String,
        done:Boolean
    }
    
    function viewItem(item){
        
        if(viewItem_model === false){
            itemInView_data = item
            viewItem_model = true
        }else if(viewItem_model === true && itemInView_data != item){
            itemInView_data = item
        }else if(viewItem_model === true && itemInView_data === item){
            viewItem_model = false
        }
        
    }


    




    let addListItem_show = false

    let newItemInput
    let newItemCommentInput

    function addListItem_model_show(){
        if (addListItem_show === false) {
            addListItem_show = true

            newItemInput.disabled = false
            newItemCommentInput.disabled = false
            newItemInput.focus()
        }else{
            addListItem_show = false
            
        }
        
        save()
    }



    

    let newItemInputcontent 

    function createItem(){
        if (newItemInput.value.length >= 1){
            let newItem = {
            title:newItemInputcontent,
            comment:newItemCommentInput.value,
            done:false
            }

            list_items.push(newItem)
            save()

            newItemInputcontent = ''
            addListItem_show = false
            newItemInput.disabled = true
            newItemCommentInput.value = ''
            newItemCommentInput.disabled = true
            }
    }



    function removeItem(item){
        _.remove(list_items, item)
        save()
    }


</script>




<style lang="scss">
    #list{
        background: #F9FAFB;
        max-width: 600px;
        padding: 10px 10px;
        border-radius: 10px;
    }

    .new-item-btn{
        background: #D1D5DB;
    }
    
    .list-items ul{
        list-style-type: none;
        padding: 0;
    }
    .list-item{
        width: 100%;
        padding: 10px;
        background: white;
        border-radius: 10px;
        transition: .3s ease;
    }

    .list-item .title{
        color: #4B5563;
        font-weight: 500;
        cursor: pointer;
    }

    .list-item .title.done-true{
        text-decoration: line-through;
    }

    .list-item.done-true{
        opacity: .5;
    }

    .list-item .item-bullet{
        width: 20px;
        height: 20px;
        border-radius: 50%;
        border: 2px solid #9CA3AF;
        cursor: pointer;
        transition: 0.1s ease;
    }
    .list-item .item-bullet.done-true{
        background: #9CA3AF;
    }


    /*
        models
    */
    .models{
        width: 100%;
        height: 100%;
        padding: 0 10px;
        position: absolute;
        left: 0;
        right: 0;
        top: 0;
        bottom: 0;
        pointer-events: none;
    }


    /*
    Add new Item
    ------------
    */
    .item_model{
        position: relative;
        max-width: 600px;
        transform: translate(0,10px);
        margin-top: 20px;
        opacity: 0;
        pointer-events: none;
        padding: 10px 20px 20px 20px;
        background: #FFFFFF;
        border: 1px solid #E5E7EB;
        box-sizing: border-box;
        box-shadow: 0px 25px 50px -12px rgba(0, 0, 0, 0.25);
        border-radius: 10px;
        transition: all .2s ease;
        z-index: 10;
    }

    .item_model input{
        width: 100%;
        padding: 10px;
        border: none;
        border-radius: 10px;
        background: #F3F4F6;;
    }

    .item_model.show-true{
        transform: translate(0,0px);
        opacity: 1;
        pointer-events: all;
    }

    .item_model .header .close{
        width: 30px;
        height: 30px;
        cursor: pointer;

    }

    .newItemCommentInput{
        width: 100%;
        padding:10px;
        min-height: 70px;
        border-radius: 10px;
        border: none;
    }
    
</style>
