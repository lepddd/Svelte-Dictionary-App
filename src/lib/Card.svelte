<script>
    export let title = 'title'
    export let desc = 'pleasure, entertainment, enjoiment, amusement, excitement.'    
</script>

<div class="container">
    <div class="container-body">
        <p class="title">{title}</p>        
        <p class="body">{desc}</p>       
    </div>
</div>

<style>
    .container{
        padding-left: 3px;
        background-color: #6242FF;
        text-align: left;
        border-radius: 5px;
    }
    .container-body{
        background-color: #fff;
        border-radius: 5px;
        padding: 5px 10px;
    }
    .title{
        color: #343A40;
        font-weight: 500;
        font-size: 16px;
    }
    .body{
        color: #6C757D;
    }

</style>