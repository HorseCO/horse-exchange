<template>
    <div>
        <table>
            <thead>
                <tr class="bg-gray-100 border-b-2 border-gray-400">
                    <th></th>
                    <th :class="{ up: this.sortOrder === 1, down: this.sortOrder === -1}">
                        <span>
                            <span class="unrderline cursor-pointer" @click="changeSortOrder">Ranking</span>
                        </span>
                    </th>
                    <th>Nombre</th>
                    <th>Precio</th>
                    <th>Cap. de Mercado</th>
                    <th>Variacion 24hrs</th>
                    <td class="hidden sm:block">
                        <input 
                            class="bg-gray-100 focus:outline-none border-b border-gray-400 py-2 px4 block"
                            id="filter"
                            placeholder="Buscar..."
                            type="text"
                            v-model="filter"
                        />
                    </td>
                </tr>
            </thead>
            <tbody>
                <tr class="border-b border-gray-200 hover:bg-gray-100" v-for="(asset, index) in filteredAssets" :key="index">
                    <td><img class="w-6 h-6" :src="`https://static.coincap.io/assets/icons/${asset.symbol.toLowerCase()}@2x.png`" :alt="asset.name"></td>
                    <td>{{ asset.rank }}</td>
                    <td>
                        <router-link 
                            class="hover:underline text-green-600"
                            :to="{name: 'coin-detail', params: {id: asset.id }}">
                            {{ asset.name }}
                        </router-link>
                        <small class="ml-1 text-gray-500">
                            {{ asset.symbol }}
                        </small>
                    </td>
                    <td>{{ asset.priceUsd | dollar }}</td>
                    <td>{{ asset.marketCapUsd | dollar }}</td>
                    <td :class="asset.changePercent24Hr.includes('-') ? 'text-red-600' : 'text-green-600'">{{ asset.changePercent24Hr | percent}}</td>
                    <td class="hidden sm:block">
                        <px-button @custom-click="goToCoin(asset.id)">
                            <span>Detalle</span>
                        </px-button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
import PxButton from '@/components/PxButton'

export default {
    name: 'PxAssetsTable',
    components: { 
        PxButton 
    },
    data(){
        return {
            filter: '',
            sortOrder: 1
        }
    },
    props: {
        assets: {
            type: Array,
            default: () => []
        },
        color: null
    },
    methods: {
        goToCoin(id){
            this.$router.push({name: 'coin-detail', params: { id }})
        },
        changeSortOrder(){
            this.sortOrder = this.sortOrder === 1 ? -1 : 1
        }
    },
    computed: {
        filteredAssets(){
            const altOrder = this.sortOrder === 1 ? -1 : 1

            return this.assets.filter(
                a =>  
                    a.symbol.toLowerCase().includes(this.filter.toLowerCase()) ||
                    a.name.toLowerCase().includes(this.filter.toLowerCase())
            )
            .sort((a, b) => {
                if(parseInt(a.rank) > parseInt(b.rank)){
                    return this.sortOrder
                }

                return altOrder
            })
        }
    }
}
</script>

<style scoped>
.up::before {
    content: '\2191'
}

.down::before {
    content: '\2193'
}
</style>