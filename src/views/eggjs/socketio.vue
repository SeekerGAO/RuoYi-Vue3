<template>
	<div class="socketio">
		<span>socketio</span>
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

socket.on("disconnect", () => {
	console.log("socket.io disconnect success");
});

socket.emit("chat", "Hello Server");
</script>

<style lang="scss" scoped></style>
vu
