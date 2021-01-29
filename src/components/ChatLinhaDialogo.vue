<template>
    <div :class="'chat ' + linha.tipo">
        {{ linha.text }}
    </div>
</template>

<script lang="ts">
import {Component, Prop, Vue } from 'vue-property-decorator';

interface LinhaDialogo {
    tipo: string;
    text: string;
}

@Component
export default class ChatLinhaDialogo extends Vue {
    @Prop() private linha!: LinhaDialogo;
}
</script>
<style >
    .chat {
	border-radius: 5px;
	box-shadow: 0 0 6px #b2b2b2;
	display: inline-block;
	padding: 10px 18px;
	position: relative;
	vertical-align: top;
}

.bot {
	align-self: flex-start;
	background: #d3d3d3;
	color: black;
	float: left;
	margin: 5px 20px 5px 45px;
}

.bot:after {
	content: '';
	position: absolute;
	left: 0;
	top: 50%;
	width: 0;
	height: 0;
	border: 20px solid transparent;
	border-right-color: #d3d3d3;
	border-left: 0;
	border-top: 0;
	margin-top: -10px;
	margin-left: -20px;
}

.me {
	align-self: flex-end;
	background: #5cb85c;
	color: white;
	float: right;
	margin: 5px 45px 5px 20px;
}

.me:after {
	content: '';
	position: absolute;
	right: 0;
	top: 50%;
	width: 0;
	height: 0;
	border: 20px solid transparent;
	border-left-color: #5cb85c;
	border-right: 0;
	border-top: 0;
	margin-top: -10px;
	margin-right: -20px;
}
</style>