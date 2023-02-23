<template>
  <div style="margin-top: 5%">
    <div style="float: right;width: 45%;display: flex;justify-content: space-between;">
      <VRow justify="">
        <div style="width: 65%;display: flex;justify-content: space-between">
          <VSelect
            label="row"
            :items="['10', '20', '30', '40']"
            variant="solo"
            style="width: 18%;"
          />
          <VSpacer />
          <VTextField
            v-model="search"
            type="text"

            label="Search"
            style="width: 70%;"
          />
        </div>
        <VSpacer />
        <VDialog
          v-model="dialog"
          persistent
          width="1024"
        >
          <template #activator="{ props }">
            <VBtn
              v-bind="props"
            >
              Add Users
            </VBtn>
          </template>
          <VCard>
            <VCardTitle>
              <span class="text-h5">User Profile</span>
            </VCardTitle>
            <VCardText>
              <VContainer>
                <VRow>
                  <VCol
                    cols="12"
                    sm="6"
                    md="6"
                  >
                    <VTextField
                      v-model="firstname"
                      label="first name"
                      required
                    />
                  </VCol>
                  <VCol
                    cols="12"
                    sm="6"
                    md="6"
                  >
                    <VTextField
                      v-model="lastname"
                      label="last name"
                    />
                  </VCol>
                  <VCol
                    cols="12"
                    sm="6"
                    md="6"
                  >
                    <VTextField
                      v-model="phonenumber"
                      label="phone"
                      persistent-hint
                      required
                      type="number"
                    />
                  </VCol>

                  <VCol
                    cols="12"
                    sm="6"
                  >
                    <VSelect
                      v-model="role"
                      :items="['User', 'Owner']"
                      label="role"
                      required
                    />
                  </VCol>
                  <VCol cols="12">
                    <VTextField
                      v-model="email"
                      label="Email"
                      required
                    />
                  </VCol>
                  <VCol cols="12">
                    <VTextField
                      v-model="password"
                      label="Password"
                      type="password"
                      required
                    />
                  </VCol>
                </VRow>
              </VContainer>
            </VCardText>
            <VCardActions>
              <VSpacer />
              <VBtn
                color="blue-darken-1"
                variant="text"
                @click="dialog = false"
              >
                Close
              </VBtn>
              <VBtn
                color="blue-darken-1"
                variant="text"
                @click="Add"
              >
                Submit
              </VBtn>
            </VCardActions>
          </VCard>
        </VDialog>
      </VRow>
    </div>
    <div style="float: left;width: 14%;display: flex;justify-content: space-between">
      <VBtn
        class="btn"
        @click="exportExecl"
      >
        export
      </VBtn>
      <VBtn
        class="btn"
        @click="Downpdf"
      >
        PDF
      </VBtn>
    </div>

    <br>
    <br>
    <br>

    <VTable>
      <thead>
        <tr>
          <th class="text-left">
            firstname
          </th>
          <th class="text-left">
            lastname
          </th>
          <th class="text-left">
            phone
          </th>
          <th class="text-left">
            email
          </th>
          <th class="text-left">
            role
          </th>
          <th class="text-left">
            status
          </th>
          <th class="text-left">
            Delete
          </th>
          <th class="text-left">
            Update
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="item in filterData"
          :key="item.id"
        >
          <td>{{ item.firstname }}</td>
          <td>{{ item.lastname }}</td>
          <td>{{ item.phonenumber }}</td>
          <td>{{ item.email }}</td>
          <td>
            <VChip
              class="ma-2"
              color="primary"
              variant="outlined"
            >
              {{ item.role }}
              <VIcon
                end
                icon="mdi-account-outline"
              />
            </VChip>
          </td>
          <td>
            <VChip
              class="ma-2"
              style="color:red;"
            >
              <VIcon
                start
                icon="mdi-account-circle-outline"
              />
              Pending
            </VChip>
          </td>
          <td>
            <VBtn
              color="primary"
              @click="Delete(item.id)"
            >
              <VIcon icon="mdi-delete" />
            </VBtn>

            <VDialog
              v-model="dialog1"
              activator="parent"
              width="auto"
            >
              <VCard>
                <VCardText>
                  Do You Want To Delete This User
                </VCardText>
                <div style="display: flex;justify-content: space-between">
                  <VCardActions>
                    <VBtn
                      color="primary"
                      @click="dialog1 = false"
                    >
                      Close
                    </VBtn>
                  </VCardActions>

                  <VCardActions>
                    <VBtn
                      color="primary"

                    >
                      Delete
                    </VBtn>
                  </VCardActions>
                </div>
              </VCard>
            </VDialog>
          </td>
          <td>
            <VBtn
              color="primary"
            >
              <VRow>
                <VDialog
                  v-model="dialog3"
                  persistent
                  width="1024"
                >
                  <template #activator="{ props }">
                    <VBtn
                      v-bind="props"
                      @click="Updates(user)"
                    >
                      <VIcon icon="mdi-file-edit-outline" />
                    </VBtn>
                  </template>
                  <VCard>
                    <VCardTitle>
                      <span class="text-h5">Update User Profile</span>
                    </VCardTitle>
                    <VCardText>
                      <VContainer>
                        <VRow>
                          <VCol
                            cols="12"
                            sm="6"
                            md="6"
                          >
                            <VTextField
                              v-model="item.firstname"
                              label="first name"
                              required
                            />
                          </VCol>
                          <VCol
                            cols="12"
                            sm="6"
                            md="6"
                          >
                            <VTextField
                              v-model="item.lastname"
                              label="last name"
                            />
                          </VCol>
                          <VCol
                            cols="12"
                            sm="6"
                            md="6"
                          >
                            <VTextField
                              v-model="item.phonenumber"
                              label="phone"
                              persistent-hint
                              required
                              type="number"
                            />
                          </VCol>

                          <VCol
                            cols="12"
                            sm="6"
                          >
                            <VSelect
                              v-model="item.role"
                              :items="['User', 'Owner']"
                              label="role"
                              required
                            />
                          </VCol>
                          <VCol cols="12">
                            <VTextField
                              v-model="item.email"
                              label="Email"
                              required
                            />
                          </VCol>
                          <VCol cols="12">
                            <VTextField
                              v-model="item.password"
                              label="Password"
                              type="password"
                              required
                            />
                          </VCol>
                        </VRow>
                      </VContainer>
                    </VCardText>
                    <VCardActions>
                      <VSpacer />
                      <VBtn
                        color="blue-darken-1"
                        variant="text"
                        @click="dialog3 = false"
                      >
                        Close
                      </VBtn>
                      <VBtn
                        color="blue-darken-1"
                        variant="text"
                        @click="updateUser"
                      >
                        Update
                      </VBtn>
                    </VCardActions>
                  </VCard>
                </VDialog>
              </VRow>
            </VBtn>
          </td>
        </tr>
      </tbody>
      <br>
      <caption>List Of Registered Users(  5 )</caption>
    </VTable>
    <div style="float: right">
      <VPagination
        v-model="page"
        :length="10"
        :total-visible="7"
        :data="users"
        @pagination-change-page="getPosts"
      />
    </div>
  </div>
</template>

<script>
import axios from "axios"
// import xlsx from "xlsx/dist/xlsx.full.min"
// import pdfMake from "pdfmake/build/pdfmake"
// import pdfFonts from "pdfmake/build/vfs_fonts"
// import router from "@/router"
//
// pdfMake.vfs = pdfFonts.pdfMake.vfs

export default {
  // eslint-disable-next-line vue/component-api-style
  data(){
    return {
      name: "user_table",
      search:'',
      page: 1,
      dialog: false,
      dialog1: false,
      dialog3: false,
      users:[],
      user: {},
      firstname: '',
      lastname: '',
      phonenumber: '',
      email: '',
      role: '',
      password: '',
      framework1:[
        ["A1", "B1", "C1"],
        ["A2", "B2", "C2"],
        ["A3", "B3", "C3"],
      ],

    }
  },
  // eslint-disable-next-line vue/component-api-style
  computed:{
    filterData(){

      return  this.users.filter(user =>user.firstname.includes(this.search))
    },
  },

  // eslint-disable-next-line vue/component-api-style
  mounted() {
    axios
      .get('http://127.0.0.1:8000/api/users')
      .then(response => (this.users = response.data))
  },

  // eslint-disable-next-line vue/component-api-style
  methods:{

    // getPosts(page = 1){
    //   axios
    //     .get('http://127.0.0.1:8000/api/users?page' + page)
    //     .then(response => (this.users = response.data))
    // },
    exportExecl(){
      const XLSX = xlsx
      const workbook = XLSX.utils.book_new()
      const worksheet = XLSX.utils.aoa_to_sheet(this.framework1)

      XLSX.utils.book_append_sheet(workbook, worksheet, "framework")
      XLSX.writeFile(workbook,"users.xlsx")
    },
    Downpdf(){
      let docDefinition = {
        content: [
          {
            layout: 'lightHorizontalLines',
            table: {
              headerRows: 1,
              widths: [ '*', "auto",100,"*"],

              body:  this.framework1,

            },
          },
        ],
      }

      // pdfMake.createPdf(docDefinition).open()
      pdfMake.createPdf(docDefinition).download()

      // pdfMake.createPdf(docDefinition).print()

    },
    async  Add(){

      try {
        const user = await axios.post(
          "http://127.0.0.1:8000/api/users",
          {
            firstname: this.firstname,
            lastname: this.lastname,
            phonenumber: this.phonenumber,
            role: this.role,
            email: this.email,
            password: this.password,
          },
        )

        console.log(user)
      } catch(e) {
        console.log(e)
      }
    },

    async  Delete(id){
      console.log(id)

      const user = await axios.delete(
        "http://127.0.0.1:8000/api/users" + id,
      )

      console.log(user)
    },
    async updateUser() {
      try {
        const user = await axios.put(
          "http://127.0.0.1:8000/api/users" + this.user.id,
          {
            firstname: this.user.firstname,
            lastname: this.user.lastname,
            phonenumber: this.user.phonenumber,
            role: this.user.role,
            email: this.user.email,
          },
        )

        console.log(user.data)
      } catch (e) {
        console.log(e)
      }
    },
    async  Updates(user){

      this.user.firstname = user.firstname
      this.user.lastname = user.lastname
      this.user.phonenumber = user.phonenumber
      this.user.role = user.role
      this.user.email = user.email
      this.user.id = user.id

    },
  },
}
</script>
