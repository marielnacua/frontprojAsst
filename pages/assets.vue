<template>

    <div>

        <NavBar />

        <div class="container">

            <h1>Asset Application</h1>

            <div class="row">

                <div class="col-6">

                    <h5>List Asset</h5>
                    <ul class="list-group">
                        <li class="list-group-item list-group-item-action" v-for="asset in assets" :key="asset.id" @click="onSelected(asset)">
                            {{asset.name}} <span class="float-right">{{asset.value}}</span>
                        </li>
                    </ul>

                </div>

                <div class="col-4">
                    <AssetView :asset="selectedAsset" @saved="onSave" @newAsset="onNew" @deleted="onDelete" />
                </div>

            </div>

        </div>

    </div>

</template>

<script>

export default {
    data() {
        return {
            assets: [],
            selectedAsset: {}
        }
    },
    methods: {
        async getAll() {
            await this.$axios.get('http://localhost:8000/api/assets')
            .then((res)=>{
                if(res.status==200) {
                    this.assets = res.data
                    console.log(this.assets)
                }
            })
        },

        onSaveo() {
            this.getAll()
            this.selectedAsset = {}
        },

        onSelected(asset) {
            this.selectedAsset = asset
        },

        onNew() {
            this.selectedAsset = {}
        },
        onDelete() {
            this.getAll()
        }
    },

    created() {
        this.getAll()
        this.selectedAsset = {}
    }
}

</script>
