<template>
    <div class="user-admin">
        <b-form>
            <input type="hidden" id="user.id" v-model="user.id" />
            <b-row>
                <b-col md="6" sm="12">
                    <b-form-group label="Nome:" label-for="user-name">
                        <b-form-input id="user-name" type="text"
                            v-model="user.name" 
                            required :readonly="mode === 'remove'"
                            placeholder="Informe o nome do usuário">
                        </b-form-input>
                    </b-form-group>
                </b-col>
                <b-col md="6" sm="12">
                    <b-form-group label="E-mail:" label-for="user-email">
                        <b-form-input id="user-email" type="text"
                            v-model="user.email" 
                            required :readonly="mode === 'remove'"
                            placeholder="Informe o E-mail do usuário">
                        </b-form-input>
                    </b-form-group>
                </b-col>
            </b-row>
            <b-form-checkbox id="user.admin" 
                v-model="user.admin" v-show="mode === 'save'" 
                class="mt-3 mb-4">
                Administrador?
            </b-form-checkbox>
            <b-row v-show="mode === 'save'">
                <b-col md="6" sm="12">
                    <b-form-group label="Senha:" label-for="user-password">
                        <b-form-input id="user-password" type="password"
                            v-model="user.password" 
                            required
                            placeholder="Informe a senha do usuário">
                        </b-form-input>
                    </b-form-group>
                </b-col>
                <b-col md="6" sm="12">
                    <b-form-group label="Cofirmar senha:" 
                        label-for="user-confirm-password">
                        <b-form-input id="user-confirm-password" type="password"
                            v-model="user.confirmPassword" 
                            required
                            placeholder="Confirme a senha do usuário">
                        </b-form-input>
                    </b-form-group>
                </b-col>
            </b-row>
            <b-button variant="primary" 
                v-if="mode === 'save'" 
                @click="save">
                Salvar
            </b-button>
            <b-button variant="danger" 
                v-if="mode === 'remove'" 
                @click="remove">
                Excluir
            </b-button>
            <b-button class="ml-2"
                @click="reset">
                cancelar
            </b-button>
        </b-form>
        <hr>
        <b-table hover striped :items="users" :fields="fields">
            <template slot="cell(actions)" slot-scope="data">
                <b-button variant="warning" @click="loadUser(data.item)" class="mr-2">
                    <i class="fa fa-pencil"></i>
                </b-button>
                <b-button variant="danger" @click="loadUser(data.item, 'remove')" class="mr-2">
                    <i class="fa fa-trash"></i>
                </b-button>
            </template>
        </b-table>
    </div>
</template>

<script>
import axios from 'axios';
import { baseApiUrl, showError } from '@/globals';

export default {
    name: 'UserAdmin',
    data: function() {
        return {
            mode: 'save',
            user: {},
            users: [],
            fields: [
                { key: 'id', label: 'Código', sortable: true },
                { key: 'name', label: 'Nome', sortable: true },
                { key: 'email', label: 'E-mail', sortable: true },
                { key: 'admin', label: 'Administrador', sortable: true,
                    formatter: value => value ? 'Sim' : 'Não' },
                { key: 'actions', label: 'Ações '}
            ]
        }
    },
    methods: {
        loadUsers() {
            axios(`${baseApiUrl}/users`).then(res => {
                this.users = res.data
            })
        },
        reset() {
            this.mode = 'save'
            this.user = {}
            this.loadUsers()
        },
        save() {
            const method = this.user.id ? 'put' : 'post'
            const id = this.user.id ? `/${this.user.id}` : ''
            axios[method](`${baseApiUrl}/users${id}`, this.user)
                .then(() => {
                    this.$toasted.global.defaultSuccess({ 
                        msg: this.user.id ? 
                        'Usuário alterado com sucesso!' :
                        'Usuário criado com sucesso'
                    })
                    this.reset()
                })
                .catch(showError)
        },
        remove() {
            const id = this.user.id
            axios.delete(`${baseApiUrl}/users/${id}`)
                .then(() => {
                    this.$toasted.global.defaultSuccess({ msg: 'Usuário excluido com sucesso!' })
                    this.reset()
                })
                .catch(showError)
        },
        loadUser(user, mode = 'save') {
            this.mode = mode
            this.user = {...user}
        }
    },
    mounted() {
        this.loadUsers()
    }
}
</script>

<style>

</style>