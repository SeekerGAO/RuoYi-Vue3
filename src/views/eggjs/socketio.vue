<template>
	<div class="socketio">
		<p @click="handleClickJoinAdmin">管理员加入</p>
		<p @click="handleClickJoinA">A加入</p>
		<p @click="handleClickJoinB">B加入</p>
		<p @click="handleClickJoinC">C加入</p>
		<p @click="handleClickAdmin">管理员</p>
		<p @click="handleClickA">这是A</p>
		<p @click="handleClickB">这是B</p>
		<p @click="handleClickC">这是C</p>
	</div>
</template>

<script setup>
import io from "socket.io-client";

const socket = io("http://localhost:7001");

socket.on("connect", () => {
	console.log("socket.io connect success");
});

socket.on("res", (data) => {
	console.log("res", data);
});

socket.on("msg", (data) => {
	console.log("msg", data);
});

socket.on("disconnect", () => {
	console.log("socket.io disconnect success");
});

socket.emit("chat", "Hello Server");
// socket.emit("join");

const handleClickJoinAdmin = () => {
	socket.emit("join", "ADMIN");
};
const handleClickJoinA = () => {
	socket.emit("join", "A");
};
const handleClickJoinB = () => {
	socket.emit("join", "B");
};
const handleClickJoinC = () => {
	socket.emit("join", "C");
};

const handleClickAdmin = () => {
	socket.emit("message", { userId: "ADMIN", msg: "我是管理员" });
};
const handleClickA = () => {
	socket.emit("message", { userId: "A", msg: "我是A" });
};
const handleClickB = () => {
	socket.emit("message", { userId: "B", msg: "我是B" });
};
const handleClickC = () => {
	socket.emit("message", { userId: "C", msg: "我是C" });
};
</script>

<style lang="scss" scoped></style>
vu
