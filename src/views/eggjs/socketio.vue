<template>
	<div class="chat-container">
		<el-row>
			<el-col>
				<p v-for="item in joinedMsgList">{{ item }}</p>
				<el-form size="large">
					<div class="flex">
						<el-select v-model="userSelected" placeholder="请选择发送对象">
							<el-option v-for="item in userOptions" :key="item.userId" :label="item.userId" :value="item.userId" />
						</el-select>
						<el-input placeholder="发送聊天消息" v-model="sendMsg" clearable> </el-input>
						<el-button type="primary" @click="handleClickSendMsg">发送</el-button>
					</div>
				</el-form>
				<div>
					<p v-for="item in msgList" :class="{ 'text-r': item.userId === 'OWN' }">
						<span>{{ item.userId }} : {{ item.msg }}</span>
					</p>
				</div>
			</el-col>
		</el-row>
	</div>
</template>

<script setup>
import io from "socket.io-client";
import { useRoute } from "vue-router";
import { ref } from "vue";

const socket = io("http://localhost:7001");

const route = useRoute();
const userId = route.query.userId.toLocaleUpperCase();
const joinedMsgList = ref([]);
const sendMsg = ref("");
const msgList = ref([]);
const userSelected = ref("");
const userOptions = ref([{ userId: "ALL" }]);

socket.on("connect", () => {
	console.log("socket.io connect success");
});

socket.on("res", (data) => {
	console.log("res", data);
});

socket.on("joined", (data) => {
	userOptions.value.push({ userId: data });
	joinedMsgList.value.push(`${data} has joined the room`);
});

socket.on("msg", (data) => {
	msgList.value.push(data);
	console.log("msg", data);
});

socket.on("disconnect", () => {
	console.log("socket.io disconnect success");
});

socket.emit("chat", "Hello Server");

// 用户加入房间
if (userId === "ADMIN") {
	socket.emit("join", "ADMIN");
}
if (userId === "A") {
	socket.emit("join", "A");
}
if (userId === "B") {
	socket.emit("join", "B");
}
if (userId === "C") {
	socket.emit("join", "C");
}

const handleClickSendMsg = () => {
	if (!sendMsg.value) return;
	msgList.value.push({ userId: "OWN", msg: sendMsg.value });
	socket.emit("message", { userId: userId, msg: sendMsg.value });
};
</script>

<style lang="scss" scoped>
.chat-container {
	padding: 50px;
}
.flex {
	display: flex;
}
.text-r {
	text-align: right;
}
</style>
vu
