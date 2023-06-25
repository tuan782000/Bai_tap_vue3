<!--- đây là cách viết cũ Option api --->
<!--- đây là thành phần hiển thị --->
<template>
    <div class="main">
        <h1 style="">Bài tập VueJS</h1>
        <!-- Bài 1 -->
        <section class="bt">
            <h2>
                Bài tập 1: Chọn ngôn ngữ lập trình: C#, JAVA, PHP, RUBY, RUST,
                GO
            </h2>
            <!-- v-bind được thay thế thành : vì vậy chỗ :key là v-bind:key -->
            <!-- cộng dụng v-for vòng lặp giúp lặp các phần tữ language bên trong mãng languages -->
            <!-- index là đánh số, tạm thời là đánh số bằng index sau này có id trả về từ api thì thay giá trị :key="id" -->
            <!-- @click là kích hoạt sự kiện và sự kiện ở đây là nghịch lại với cái giá trị hiện tại sau khi click
                                        => language.selected = true or false
             -->
            <!-- v-bind:class => :class "trong đây là giá trị class" 
                  
                pick: language.selected => pick: true thì hiển thị pick
                                        => pick: flase thì 0 hiển thị pick
             -->
            <span
                v-for="(language, index) in languages"
                :key="index"
                @click="language.selected = !language.selected"
                :class="{ pick: language.selected }"
            >
                {{ language.name }}
                <!-- {{language.selected}}   -->
            </span>
        </section>
        <!-- Bài 2 -->
        <section class="bt">
            <h2>Bài tập 2: Tạo Modal</h2>
            <!-- Tạo ra nút gán sự sự kiện on -->
            <!-- @click: kích hoạt sự kiện biến open thành true -->
            <button class="create-modal" @click="open = true">
                Open Modal
            </button>
            <!-- v-if: để xét điều kiện true thì tạo ra element (Modal) đó, false thì xóa element đó-->
            <div v-if="open" class="modal">
                <p>Hello from the modal!</p>
                <!-- @click biến open thành false -->
                <button class="btn-close" @click="open = false">Close</button>
            </div>
        </section>
        <!-- bài 3 -->
        <section class="bt">
            <h2 class="bt3">Bài tập 3: Chọn tất cả, bỏ chọn tất cả</h2>
            <div>
                <table>
                    <tr>
                        <th>Name</th>
                        <th>Email</th>
                        <th>
                            Select
                            <input
                                type="checkbox"
                                @click="toggleSelect"
                                :checked="selectAll"
                            />
                        </th>
                    </tr>
                    <!-- chưa biết thay index bằng id -->
                    <tr v-for="(user, index) in users" :key="index">
                        <td>{{ user.name }}</td>
                        <td>{{ user.email }}</td>
                        <td>
                            <input type="checkbox" v-model="user.checked" />
                        </td>
                    </tr>
                </table>
            </div>
        </section>
        <!-- Bài 4 Dual Listbox -->
        <section class="bt">
            <h2 class="bt3">Bài tập 4: Dual Listbox</h2>
            <div id="select">
                <div>
                    <student-list :students="students" />
                </div>
                <div>
                    <button @click="moveToRight()" class="toLeft">Sang Phải</button>
                    <br />
                    <button @click="moveToLeft()" class="toRight">Sang Trái</button>
                </div>
                <div>
                    <student-list :students="userSelectedStudents" />
                </div>
            </div>
        </section>
    </div>
</template>
<!--- đây là thành phần xử lý --->
<script>
import StudentList from "./components/StudentList.vue";
export default {
    components: { StudentList },
    data() {
        return {
            // Bài tập 1
            // Đầu tiên mình tạo ra 2 mãng, mãng 1 chứa các language và mãng 2 chứa các language đã chọn
            // Trong mãng languages sẽ chứa các đối tướng language
            languages: [
                { name: "C#", selected: false },
                { name: "PHP", selected: false },
                { name: "GO", selected: false },
                { name: "RUBY", selected: false },
                { name: "JAVA", selected: false },
            ],
            selectedLanguages: [],

            // Bài tập 2:
            // giá trị ban đầu phải là false để ẩn đi Modal
            open: false,

            // Bài tập 3:
            // Chọn 1 hoặc tất cả user
            users: [
                {
                    id: "1",
                    name: "Tuan",
                    email: "Tuan@yahoo.com",
                    checked: false,
                },
                {
                    id: "2",
                    name: "Nguyen",
                    email: "Nguyen@yahoo.com",
                    checked: false,
                },
                {
                    id: "3",
                    name: "Thai",
                    email: "Thai@yahoo.com",
                    checked: false,
                },
            ],

            // Bài tập 4: Dual List Box
            students: [
                { name: "Tuan", selected: false },
                { name: "Nguyen", selected: false },
                { name: "Thai", selected: false },
                { name: "Peter", selected: false },
                { name: "John", selected: false },
            ],
            userSelectedStudents: [],
        };
    },
    computed: {
        // Này là của bài tập 3
        selectAll: function () {
            return this.users.every(function (user) {
                return user.checked;
            });
        },
    },
    methods: {
        // này của bài tập 3
        toggleSelect: function () {
            var select = this.selectAll;
            this.users.forEach(function (user) {
                user.checked = !select;
            });
            this.selectAll = !select;
        },
        // Này là của bài tập 4
        moveToRight() {
            // đoạn này test thử
            console.log(this.students);

            // sử dụng hàm filter() để lọc ra các danh sách sinh viên có selected
            // Lấy ra những thằng được chọn gán nó vào biến
            let stuentsToMoveRight = this.students.filter((s) => s.selected);
            console.log(stuentsToMoveRight);
            // Lấy ra những thằng không được chọn
            this.students = this.students.filter((s) => s.selected == false);
            // đẩy những thằng được chọn vào mãng userSelectedStudents
            // this.userSelectedStudents.push(stuentsToMoveRight) thằng này chỉ đẩy qua mãng mới (push)) được một cái thui
            // Phương án đẩy nhiều cái dùng spread operator, cái ...
            this.userSelectedStudents.push(...stuentsToMoveRight);
            // Qua phải xong thì phải bỏ chọn
            this.userSelectedStudents.forEach((s) => {
                s.selected = false;
            });
        },

        // copy tương tự trên cho hàm di chuyển sang lại bên trái
        moveToLeft() {
             console.log(this.userSelectedStudents);
            let stuentsToMoveLeft = this.userSelectedStudents.filter((s) => s.selected);
            this.userSelectedStudents = this.userSelectedStudents.filter((s) => s.selected == false);
            //đẩy phần từ mãng trái qua mãng phải || this.students đại diện cho mãng phải
            this.students.push(...stuentsToMoveLeft);
            this.students.forEach((s) => {
                s.selected = false;
            });
        },
    },
};
</script>
<!--- đây là thành phần css --->
<style scoped>
.main {
    background-color: orange;
    height: 100%;
    font-family: sans-serif;
}
.bt {
    margin: 50px;
}
span {
    display: inline-block;
    margin: 30px;
    padding: 20px 30px;
    border: 1px solid #000;
}
.pick {
    background-color: green;
    color: white;
    padding: 20px 30px;
}
.create-modal {
    background: green;
    color: white;
    border: none;
    padding: 10px 20px;
    margin: 10px;
}
.modal {
    position: fixed;
    z-index: 999;
    top: 20%;
    left: 50%;
    width: 300px;
    margin-left: -150px;
    background-color: white;
    width: 300px;
    height: 100px;
    border-radius: 5px;
}

.modal p {
    text-align: center;
    font-size: 30px;
}
.btn-close {
    position: absolute;
    border: none;
    background-color: red;
    color: white;
    padding: 10px 20px;
    bottom: 5px;
    right: 5px;
    border-radius: 5px;
}
table,
th,
td {
    border: 1px solid black;
    padding: 10px 0;
}
table {
    width: 50%;
    margin: 0 auto;
    text-align: center;
    border-collapse: collapse;
}
.bt3 {
    margin-bottom: 30px;
}

.dualListBox {
    display: flex;
    gap: 10px;
}

#select {
    display: flex;
    row-gap: 10px;
    justify-content: center;
}
#select div {
    border: 1px solid #000;
    padding: 15px;
}
</style>
