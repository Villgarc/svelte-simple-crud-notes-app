<script >
    export let status = "";
    export let msg = "";
    function getStatus (status){
        if (status == "warning") return "alert alert-warning alert-dismissible fade show"
        if(status == "info") return "alert alert-success alert-dismissible fade show"
        if(status == "deletion") return "alert alert-danger alert-dismissible fade show"
    }
</script>

<div class="msg">
    {#if status}
        <div
            class = "{getStatus(status)}"
            role="alert"
        >
            {msg}
           
        </div>
    {/if}
</div>
