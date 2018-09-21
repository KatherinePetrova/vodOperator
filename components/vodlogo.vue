<template>
	<div class="basic">
		<h1>vodOperator</h1>
	</div>
</template>

<style>
	.basic { 
		font-size: 5em;
		color: rgba(102, 255, 102, 0.8);
		letter-spacing: 1px;
		text-shadow: 0 60px rgba(166, 166, 166, 0.8),
               		 0 -60px rgba(140, 255, 102, 0.8);
        margin-top: 1em;
        margin-bottom: 1em;
        animation-name: logo;
        animation-duration: 2s;
        animation-timing-function: linear;
	}

	@keyframes logo{
		0%{text-shadow: 0 0 rgba(166, 166, 166, 0.8),
               		 0 0 rgba(140, 255, 102, 0.8);}
        100%{text-shadow: 0 60px rgba(166, 166, 166, 0.8),
               		 0 -60px rgba(140, 255, 102, 0.8);}
	}
</style>