<template>

    <div>

        <button class="btn btn-success float-right" @click="onNew">
            New Asset
        </button>

        <h5>Asset View</h5>

        <hr>

        <form action="#" @submit.prevent="onSave">
            <b-form-group
                label="Name"
                label-for="laptop_name"
                >
                <b-form-input id="laptop_name" v-model="asset.name" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Description"
                label-for="description"
                >
                <b-form-input id="description" v-model="asset.description" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Location"
                label-for="location"
                >
                <b-form-input id="location" v-model="asset.location" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Value"
                label-for="value"
                >
                <b-form-input id="value" v-model="asset.value" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Date"
                label-for="acquired_on"
                >
                <b-form-input id="acquired_on" type="date" v-model="asset.acquired_on" trim></b-form-input>
            </b-form-group>

            <b-form-group
                label="Status"
                label-for="status"
                >
                <b-form-select id="status" v-model="asset.status" :options="statuses"></b-form-select>
            </b-form-group>

            <b-form-group>
                <button class="btn btn-primary" type="submit">Save</button>
                <button class="btn btn-danger" type="button" @click="onDelete" v-if="asset.id">Delete</button>
            </b-form-group>
                
        </form>

    </div>

</template>

<script>

export default {
    props: {
        asset: {}
    },
    data() {
        return {
            asset: {},
            statuses: [
                { value: 'good', text: 'Good' },
                { value: 'very good', text: 'Very Good' },
                { value: 'excellent', text: 'Excellent' },
                
            ]
        }
    },
    methods: {
        async onSave() {
            try {

                if(!this.asset.id) {
                    this.$axios.post('http://localhost:8000/api/assets', this.asset)

                }else{
                    this.$axios.put('http://localhost:8000/api/assets/' + this.asset.id, this.asset)

                }

                this.$emit('saved');

            } catch (error) {
                alert(error.response.data.message)
            }
        },

        onNew() {
            this.$emit('newAsset')
        },

        async onDelete() {
            try {
                this.$axios.delete('http://localhost:8000/api/assets/' + this.asset.id)

                this.$emit('deleted')
                
            } catch (error) {
                alert(error.response.data.message)
            }
            
        }
    }
}

</script>