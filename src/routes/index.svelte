

<div class="container mt-5">
    
<div id="list" class="list_container">
    <!--add item model-->
    <div class="add-list-item show-{addListItem_show}">
        <div class="header d-flex justify-content-between mb-2">
            <span></span>
            <div on:click="{addListItem_model_show}" class="bg-danger rounded-circle close d-flex justify-content-center align-items-center"><img src="/x-circle.svg" alt="close icon"></div>
        </div>
        <form action="#" class="d-flex flex-wrap align-items-center flex-sm-row  flex-column">
            <input id="listTitle" type="text" bind:this="{newItemInput}" bind:value="{newItemInputcontent}"  placeholder="Enter Title" class="input me-1">
            <button on:click|preventDefault ="{createItem}" class="btn btn-light mt-3  mt-sm-0">Add</button>
        </form>
    </div>
    <!--add item /model-->
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
                            <span class="title d-flex align-items-center flex-wrap done-{item.done}">
                                <div on:click="{checkOut(item)}" class="item-bullet me-2 done-{item.done}"></div>
                                {item.title}
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

    let list_items = [
        {
            title:"Merge the new branch with master.",
            comment:"",
            done:false
        }
    ]

    function checkOut(item){
        if (item.done === false){
            item.done = true
            list_items = list_items
        }else{
            item.done = false
        }
        list_items = list_items
    }



    let addListItem_show = false

    let newItemInput

    function addListItem_model_show(){
        if (addListItem_show === false) {
            addListItem_show = true
            newItemInput.disabled = false
            newItemInput.focus()
        }else{
            addListItem_show = false
            
        }
        
        addListItem_show = addListItem_show
    }



    

    let newItemInputcontent 

    function createItem(){
        if (newItemInput.value.length >= 1){
            let newItem = {
            title:newItemInputcontent,
            comment:"",
            done:false
            }

            list_items.push(newItem)
            list_items = list_items
            newItemInputcontent = ''
            addListItem_show = false
            newItemInput.disabled = true
            }
    }



    function removeItem(item){
        _.remove(list_items, item)
        list_items = list_items
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
    Add new Item
    ------------
    */
    .add-list-item{
        position: absolute;
        left: 50%;
        transform: translate(-50%,20px);
        opacity: 0;
        pointer-events: none;
        max-width: 300px;
        padding: 10px 20px 20px 20px;
        background: #FFFFFF;
        border: 1px solid #E5E7EB;
        box-sizing: border-box;
        box-shadow: 0px 25px 50px -12px rgba(0, 0, 0, 0.25);
        border-radius: 10px;
        transition: .2s ease;
        z-index: 10;
    }

    .add-list-item input{
        padding: 10px;
        border: none;
        border-radius: 10px;
        background: #F3F4F6;;
    }

    .add-list-item.show-true{
        transform: translate(-50%,0px);
        opacity: 1;
        pointer-events: all;
    }

    .add-list-item .header .close{
        width: 30px;
        height: 30px;
        cursor: pointer;

    }

    
</style>
