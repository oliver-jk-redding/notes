#Notes on using CSS, SASS, LESS, etc

##Aligning divs in pretty rows and columns without floats and flexbox (2 across)
```bash
.container {
	width: 100%;
	font-size: 0;

	> .inner-divs {
		margin-right: 2%;

		display: inline-block;
		vertical-align: top;
		width: 49%;
	}

	.inner-divs:nth-child(2n+0) {
		margin-right: 0;
	}
}
```