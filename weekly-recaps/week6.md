# HTML and CSS Lessons

## CSS Background Images
Last week, we built a set of tables that showcased information about the rankings of other movies. We added ratings, but we need a visual cue so people can scan for well-rated movies. To do this, we will add certified fresh, fresh, and rotten symbols as background images. 

Setting backgrounds in CSS is similar to setting fonts—there is a shorthand syntax for `background`, and an expanded list of settings such as `background-image` and `background-repeat` that allow us to change specific things about the background. For our purposes, we want to define a 16 by 16 pixel image as a background that does not repeat, is vertically centered and aligned to the left, and we want that image to be different for three different types of ratings. First, let’s express the classes that we want to target. The unmodified class for these elements is `.movieListing-rating`. Let’s set the shared properties on this element:

```
.movieListing-rating {
    background-repeat: no-repeat;
    background-size: 16px;
    background-position: center left;
}
```

This tells the background how big it should be, where it should be, and whether it should repeat. Let’s add some modified classes for targeting with our specific background images. We will define the classes of `.movieListing-rating--certified`, `.movieListing-rating--fresh`, and `.movieListing-rating—rotten`. Let’s see how we can set background images for these:

```
.movieListing-rating--certified {
    background-image: url(../images/CF_16x16.png);
}

.movieListing-rating--fresh {
    background-image: url(../images/fresh-16.png);
}

.movieListing-rating--rotten {
    background-image: url(../images/splat-16.png);
}
``` 

There’s one more thing we need to do, which is to leave space for the images within their table cells. We add one final rule to the base class:

```
.movieListing-rating {
    padding-left: 2rem;
}
```

Now we have defined our background images. What if this information on certified, fresh, and rotten is being used in javascript as well? As we discussed before with advanced CSS selectors, we can match rules against elements with specific attributes. For example, we could define a `movieListing-rating` with a `data-rating` attribute:

```
<td class=“movieListing-rating” data-rating=“certified”>
```

And then in CSS:

```
.movieListing-rating[data-rating="certified"] {
    background-image: url(../images/CF_16x16.png);
}
```

This is just one option for targeting this sort of information. Once we begin to use [LESS](http://lesscss.org) we will begin introducing some more ways we can make use of these sorts of attributes to apply styles. 


## Complex Module Layouts

### Outlining
The top section is a complex layout featuring multiple columns and rows. How can we faithfully reproduce this section of the page? First, we will go through and outline the basic structure of the content. We may change these tags later on, but we are going to start by outlining most things as divs. 

```
<section class="movie-overview">
    <div class="movie-poster">
        <img src="images/11179312_det.jpg">
        <button class="btn btn--tickets">Tickets and Showtimes</button>
    </div>
    <div class="movie-reviewInfo">
        <div>
            <div class="tomatometer">
                <h3>Tomatometer</h3>
                <p>8%</p>
                <dl>
                    <dt>Average Rating</dt>
                    <dd>3.3/10</dd>
                    <dt>Reviews Counted</dt>
                    <dd>65</dd>
                    <dt>Fresh</dt>
                    <dd>5</dd>
                    <dt>Rotten</dt>
                    <dd>60</dd>
                    <dt>Critics Consensus</dt>
                    <dd>Slowly, steadily, although no one seems to be moving it in that direction, the Ouija planchette points to NO.</dd>
                </dl>
            </div>
            <div class="audienceScore">
                <h3>Audience Score</h3>
                <p>33% <span>liked it</span></p>
                <dl>
                    <dt>Average Rating</dt>
                    <dd>2.7/5</dd>
                    <dt>User Ratings</dt>
                    <dd>17,022</dd>
                </dl>
            </div>
        </div>
        <div>
            <div class="trailer">
                <h3>Trailer</h3>
                <div>
                    <figure>
                        <img src="images/918569_031.jpg">
                        <figcaption>HD Video</figcaption>
                        <a href="#"></a>
                    </figure>
                </div>
            </div>
            <div class="photos">
                <h3>Photos</h3>
                <img src="images/thumbnail1.jpg" alt=""/>
                <img src="images/thumbnail2.jpg" alt=""/>
                <img src="images/thumbnail3.jpg" alt=""/>
                <img src="images/thumbnail4.jpg" alt=""/>
                <img src="images/thumbnail5.jpg" alt=""/>
                <img src="images/thumbnail6.jpg" alt=""/>
                <img src="images/thumbnail7.jpg" alt=""/>
                <img src="images/thumbnail8.jpg" alt=""/>
            </div>
        </div>
    </div>
</section>
```

This might seem a little overwhelming, so let’s break it down a bit. First, we have identified the main sections of the document - the poster, the tomatometer, the audience score, the trailer, and the photos. We are ignoring for now the section for adding a  rating, because we are going to talk about forms at a later date. 


### Defining multiple columns
Let’s first start by breaking this into two columns. We have the left column that includes the movie poster and the tickets button, and we have the right column that hosts everything else. We can model these sections as `movie-poster` and `movie-reviewInfo`, which will serve as the top level identifiers for these sections. From there, we can add sizing to the columns. We will define the left column as 25% and the right column as 75%, with a couple percentage reserved for padding.

```
.movie-poster, .movie-reviewInfo {
    float: left;
}

.movie-poster {
    width: 25%;
}

.movie-reviewInfo {
    width: 75%;
    padding-left: 2%;
}
```

Remember that in order for these rules to work, you need to have content set to `box-sizing: border-box;`. This will cut the padding out of the 75%, rather than adding to it.

### Button styles
Now that we have two columns, we can style the first column. Our poster already looks the way it should in the space, thanks to our rule that defines `max-width: 100%` on all `<img>` tags. Let’s move down to the button. We have yet to style the buttons on our website yet, so we can go ahead and work on defining a base class of `.btn` before creating a special class for this individual button. The default style for a button varies wildly across devices and browsers, so it is important to be explicit in our overrides when using the `<button>` tag. One workaround for this is to use `<a class=“btn”>`, but we won’t need to do that here. 

We will crib the button colors from the live website so we don’t have to guess at the hex codes. Let’s start by adding just the colors.

```
.btn {
    background-color: #0C89CA;
    color: white;
    border-color: #0b79b2;
}
```

The outcome of this will look something like:
<a style=“background-color: #0C89CA; color: white; border-color: #0b79b2;”>Example button</a>

This isn’t a great result yet, and it remains inconsistent across browsers. Let’s add some padding and increase the font size: 

```
.btn {
    background-color: #0C89CA;
    color: white;
    border-color: #0B79B2;
    font-size: 1.4rem;
    padding: 0.5rem 1rem;
}
```

This is closer, but it is missing two key things compared to the original - an inset shadow and rounded corners. Previously, these would require complicated workarounds, like an image for each corner, but they are now easy thanks to CSS3. To define a shadow, we can use the `box-shadow` property. The default is a drop shadow, but we can also use it for inset shadows. The only difference is to start with the `inset` keyword. Let’s see what the `box-shadow` rule for this element looks like: 

```
box-shadow: inset 0 1px #68B6DE;
```

This may look confusing at first, but the two numbers simply define the x- and y-offsets for the shadow (relative to the upper left corner of the button). The last item is just the shade of the shadow. There are two additional properties that can be added as well, which are the blur and the spread of the shadow. You can play around with these properties in a [JSFiddle](http://jsfiddle.net). You can choose to specify a fallback style for pre-CSS3 browsers, or you can simply let it degrade gracefully and not apply the shadow on those browsers at all—they will simply ignore the rule.

The other property that we need to add is called `border-radius`, and the syntax is similar to other properties such as `padding` and `margin`. In our case, because our corners will all have the same radius, our rule is simple:

```
border-radius: 5px;
```

Putting this all together, our new button definition looks something like this: 

```
button, .btn {
    background-color: #0C89CA;
    color: white;
    border-color: #0B79B2;
    font-size: 1.4rem;
    box-shadow: inset 0 1px #68B6DE;
    border-radius: 5px;
    padding: 0.5rem 1rem;
}
```

<a style=“background-color: #0C89CA; color: white; border-color: #0B79B2; font-size: 1.4rem; box-shadow: inset 0 1px #68B6DE; border-radius: 5px; padding: 0.5rem 1rem;”>Example button</a>



