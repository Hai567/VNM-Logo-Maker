<script>
	import domtoimage from 'dom-to-image';
	let brandName = 'Vinamilk';
	let establishAbbreviation = 'EST';
	let establishedYear = '2023';
	let logoNode;
	let downloadLinkNode;
	let logoType;
	let roundedCorner;
	let size = 10;
	$: resultPadding = 3;
	$: lowerInfoSize = 2;
	let changeSize = () => {
		if (size <= 3) {
			resultPadding = 0.5;
			lowerInfoSize = 0.5;
		} else if (size <= 6) {
			resultPadding = 1;
			lowerInfoSize = 1;
		} else if (6 < size <= 14) {
			resultPadding = 2;
			lowerInfoSize = 2;
		} else if (14 < size) {
			resultPadding = 3;
			lowerInfoSize = 3;
		}
	};
	$: borderRadius = 0;
	let generateLogo = () => {
		domtoimage
			.toPng(logoNode)
			.then(function (dataUrl) {
				var img = new Image();
				img.src = dataUrl;
				downloadLinkNode.href = dataUrl;
				downloadLinkNode.click();
			})
			.catch(function (error) {
				console.error('oops, something went wrong!', error);
			});
	};
</script>

<svelte:head
	><link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
	<link
		href="https://fonts.googleapis.com/css2?family=Roboto:wght@500;700;900&display=swap"
		rel="stylesheet"
	/>
</svelte:head>
<div class="intro">
	<h2>Please put your phone into landscape mode to have the best experience!</h2>
	<img src="/imgs/rotate-phone.gif" alt="">
</div>
<main class="container">
	<form action="" class="input-form">
		<div class="input-part">
			<label for="establish-abbreviation">Establish Abbreviation: </label>
			<input
				id="establish-abbreviation"
				bind:value={establishAbbreviation}
				type="text"
				placeholder="Establish Abbreviation"
			/>
		</div>
		<div class="input-part">
			<label for="brand-name">Brand Name: </label>
			<input id="brand-name" bind:value={brandName} type="text" placeholder="Brand Name" />

			<label for="size">Size: </label>
			<select name="" id="size" bind:value={size} on:change={changeSize}>
				{#each { length: 18 } as _, i}
					<option value={i + 1}>{i + 1}</option>
				{/each}
			</select>
		</div>
		<div class="input-part">
			<label for="established-year">Established Year: </label>
			<input
				id="established-year"
				bind:value={establishedYear}
				type="text"
				placeholder="Established Year"
			/>
		</div>
		<div class="input-part">
			<label for="logo-type">Logo Type: </label>
			<select name="logo-type" id="logo-type" bind:value={logoType}>
				<option value="default" selected>Default</option>
				<option value="inverted">Inverted</option>
			</select>
		</div>
		<div class="input-part">
			<label for="rounded-corner">Rounded Corner: </label>
			<select
				bind:value={roundedCorner}
				name="rounded-corner"
				id="rounded-corner"
				disabled={logoType === 'default'}
			>
				<option value={true}>Nahh, for what?</option>
				<option value={false}>Yepp, use them!</option>
			</select>
		</div>
		<div class="input-part">
			<label for="border-radius">Rounded Corner Radius: </label>
			<select
				name=""
				id="border-radius"
				disabled={roundedCorner === true}
				bind:value={borderRadius}
			>
				{#each { length: 500 } as _, i}
					<option value={i + 10}>{i + 10}px</option>
				{/each}
			</select>
		</div>
	</form>
	<div class="result-container">
		<div
			class="result"
			class:inverted-logo={logoType === 'inverted'}
			bind:this={logoNode}
			style="border-radius: {borderRadius}px; padding: {resultPadding}em;"
		>
			<h1 style="font-size: {size}em;">{brandName}</h1>
			<div class="lower-info" style="font-size: {lowerInfoSize}em;">
				<span class="establishedAbbreviation">{establishAbbreviation}</span>
				<span class="establishedYear">{establishedYear}</span>
			</div>
		</div>
	</div>
	<form class="download-form" action="" on:submit|preventDefault={generateLogo}>
		<a bind:this={downloadLinkNode} href="" class="download-link" download
			><button>Download</button></a
		>
	</form>
</main>

<style>
	.intro{
		width: 100%;
		height: 100vh;
		background-color: black;
		position: absolute;
		top: 0;
		left: 0;
		justify-content: center;
		flex-wrap: wrap;
		transition: all .4s ease;
		font-family: "Roboto";
	}
	.intro h2{
		margin-top: 4em;
		color: white;
		text-align: center;
	}
	.intro img{
		width: 310px;
		position: absolute;
		top: 0;
		bottom: 0;
		margin: auto;
	}
	@media screen and (orientation: portrait) {
		.intro {
			display: flex;
		}
		main.container{
			display: none;
		}
	}
	/* Landscape orientation */
	@media screen and (orientation: landscape) {
		.intro {
			position: absolute;
			top: -100vh;
		}
		main.container{
			display: block;
		}
	}
	.download-form {
		display: flex;
		justify-content: center;
	}
	.download-form button {
		width: 12em;
		height: 4em;
	}
	.input-form {
		display: flex;
		flex-wrap: wrap;
		gap: 14px;
		justify-content: center;
	}
	.input-part {
		width: 15em;
		text-align: center;
	}
	.input-part label {
		display: block;
	}
	.inverted-logo {
		background-color: #0213b0 !important;
		color: white !important;
	}
	@font-face {
		font-family: 'VNMSansDisplayBold';
		src: url('/fonts/VNMSansDisplay/VNM_Sans_Display_Bold.otf');
	}
	/* @font-face {
		font-family: 'VNMSansDisplayRegular';
		src: url('/fonts/VNMSansDisplay/VNM_Sans_Display_Regular.otf');
	}
	@font-face {
		font-family: 'VNMSansDisplayStandard';
		src: url('/fonts/VNMSansDisplay/VNM_Sans_Display_Std.otf');
	} */
	.container {
		padding-top: 120px;
		width: 80%;
		margin: auto;
		font-family: 'Roboto';
		transition: all .4s ease;
	}
	.result-container {
		display: flex;
		justify-content: center;
		font-family: 'VNMSansDisplayBold' !important;
		padding: 4em;
		user-select: none;
	}
	.result {
		color: #0213b0;
		padding: 3em;
		background-color: white;
	}
	.result h1 {
		margin: 0;
		font-size: 10em;
		line-height: 0.7em;
	}
	.result span {
		margin: 0;
		font-size: 2em;
	}
	.lower-info {
		display: flex;
		justify-content: space-between;
	}
</style>
