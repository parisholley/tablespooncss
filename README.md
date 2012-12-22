# tablespoon

Sassy and simple responsive design using CSS tables. Look ma, NO GRID!

## Philosophy

### No Math, No Gutters, No "Grid" Just Simple

Every CSS/Grid/Responsive framework in my opinion is just too complicated and limiting.

* Want a mixture of fixed AND fluid columns? Nope!
* What 2 columns in header, 5 in the content area and 4 in the footer? Nope! (unless you want to have a 40 (2*5*4) column grid)
* Want to center verticly without hacks? Nope!
* Want to calculate optimal widths, gutters, padding, golden ratio and the circumference of the earth in your head? Nope! Get a calculator.

In this design, I go with the idea that the "grid"/table is PURE layout. If you need padding or gutters, add it to your content. It isn't neccessary to design your layout around the idea that columns can be moved around at will. How often do you really do that?

Keep your grid simple, nice even numbers/percentages and add spacing when it is needed, otherwise you will fight frameworks when you want to use the full width of the column;

### No Markup

From what I have seen, all frameworks that live outside of SASS require markup. Here is the progression of styling over the years:

1. Tables, auto-height, styling for the win!
2. Tables defy the semantic gods and are for n00bs, use divs! 
3. I'm sippin on my margarita, sitting on a floatie, look how my divs resize with the browser!
4. w00t, media queries, lets make CSS complicated: RESPONSIVE! GRIDS! LIGHTS! BUTTONS! MOOOOOOBILLLLLLEEEEE!
5. I just want this thing to work on mobile.

I am at #5 at this point. A designer provides a PSD, and it needs to look good. What major problems do these frameworks truly solve that is worth the complication?


### box-sizing: border-box

This in my opinion reinforces simplicity. If I say the width is 1000px, it is a 1000px, regardless of padding, borders, etc. You can always wrap a container and style it or place padding on the actual content, and not your layout.

## Behavior

### Mobile or Desktop Only

Currently, I had no need to customize the design based on tablets, so it makes this library very simple. All definitions by default apply to the desktop and mobile will fallback to a single column design. HOWEVER, there is nothing stopping you from using media queries on columns and optimizing further on a table, this library will not lock you into a grid.

### Fixed, then Fluid

By default, any widths/percentages passed into mixins will ONLY apply to the desktop. Any table that is defined as a fixed width will become fluid soon as it becomes to big to display in the browser.

## How To Use

I assume at this point you know how to setup a SASS project, if not, im sorry, just too lazy to tell you how.

### Import

Currently, you just need to drop this repo into a folder (call it "tablespoon"?) that is relative to your other .scss files and include this:

	@import "tablespoon/tablespoon";

At some point I will turn this into a ruby GEM.

### Examples

#### Fixed Widths
	#wrapper{
		@include table(1000px);

		#col1{
			@include column;
			width: 700px;
		}

		#col2{
			@include column;
			width: 300px;
		}
	}

#### Fluid Widths
	#wrapper{
		@include table(1000px);

		#col1{
			@include column;
			width: 70%; // 700px
		}

		#col2{
			@include column;
			width: 30%; // 300px
		}
	}	

#### Hybrid Widths
	#wrapper{
		@include table(1000px);

		#col1{
			// because no width is defined, this will be fluid.
			@include column;
		}

		#col2{
			@include column;
			width: 300px;
		}
	}		

#### Nested Columns
	#wrapper{
		@include table(1000px);

		#col1{
			// because no width is defined, this will be fluid.
			@include column;

			#content{
				@include table; //defaults to 100%

				#left{
					@include column;
					width: 25%;
				}

				#right{
					@include column;
					width: 75%;
				}
			}
		}

		#col2{
			@include column;
			width: 300px;
		}
	}

#### Media Query Widths
In this example, I can define my own behavior based on the device width without being locked into a grid layout or HTML markup.

	#wrapper{
		@include table(1000px);

		#col1{
			@include column;
		}

		#col2{
			@include column;

			@media screen and (max-width: 768px) {
				width: 200px;
			}

			@media screen and (min-width: 769px) {
				width: 300px;
			}
		}
	}

#### Padding Wrapper (All Devices)
	#wrapper{
		@include table(1000px); // effective width is 960px due to padding

		padding-left: 20px;
		padding-right: 20px;
	}

#### Padding Wrapper (Only on Desktop)
I would recommend not going with this approach as typically a slight padding looks good on every width (even mobile), but perhaps you want a smaller amount when on mobile.

	#wrapper{
		padding-left: 10px; // applies to mobile
		padding-right: 10px;

		@include table(1000px){
			padding-left: 20px;
			padding-right: 20px;
		}
	}

#### Padding Column (Only on Desktop)
This example shows how you could have right rail design that has space between the main content, but only applies on the desktop because when on mobile this will be a single column layout and a 20px padding will look odd.

	#wrapper{
		@include table(1000px);
		padding: 0 20px;

		#left{
			@include column;
		}

		#right{
			@include column{
				padding-left: 20px;
			}

			width: 300px;
		}
	}

#### Phone/Desktop Targeting (Approach 1)
	#wrapper{
		display: none;

		@include desktop{
			display: block;
		}
	}

#### Phone/Desktop Targeting (Approach 2)
	#wrapper{
		display: block;

		@include mobile{
			display: none;
		}
	}

#### Phone/Desktop Targeting (Approach 3)
	#wrapper{
		@include desktop{
			display: block;
		}

		@include mobile{
			display: none;
		}
	}
