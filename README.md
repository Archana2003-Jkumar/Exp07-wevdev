# Ex-07:
## Image Carousel in react using hooks
### AIM:
The aim of this code is to create an image carousel using React hooks. The carousel displays a set of images with corresponding titles and descriptions. Users can navigate through the images using arrow buttons and dots indicators.
### ALGORITHM:
1. Create an array of objects called "SliderImage", where each object represents an image slide with properties like title, description, and URL of the image.
2. Create separate components for the arrows, dots, and the main slider content.
3. In the Slider component, initialize the activeIndex state variable using the useState hook to track the currently displayed slide.
4. Use the useEffect hook to set up an interval that automatically updates the activeIndex to the next slide after a certain duration.
5. Implement functions for the previous and next slide actions, updating the activeIndex accordingly.
6. Render the UI elements using JSX:
* Wrap the slider content, arrows, and dots components inside a container.
* Pass the activeIndex and the SliderImage array as props to the slider content, dots, and arrows components.
*Attach event handlers to the arrow buttons and dots indicators to update the activeIndex.
7. Apply CSS styles to achieve the desired layout and appearance of the carousel.
### PROGRAM:
### SliderImage.js:
javascript
import first from "../assets/first.jpg";
import second from "../assets/second.jpg";
import third from "../assets/third.jpg";

export default[
    {
        title: "FIRST SLIDE",
        description: "Welcome to the first slide of the carousel",
        urls: first,
    },
    {
        title: "SECOND SLIDE",
        description: "Welcome to the second slide of the carousel",
        urls: second,
    },
    {
        title: "THIRD SLIDE",
        description: "Welcome to the third slide of the carousel",
        urls: third,
    },
];

### Arrows.js
javascript
import React from "react";

function Arrows({prevSlide, nextSlide}){
    return(
        <div className="arrows">
            <span className="prev" onClick={prevSlide}>
                &#10094;
            </span>
            <span className="next" onClick={nextSlide}>
                &#10095;
            </span>
        </div>
    );
}

export default Arrows;

### Dots.js
javascript
import React from "react";

function Dots ({activeIndex, onClicks, sliderImage}){
    return(
        <div className="all-dots">
            {sliderImage.map((slide, index) => (
                <span
                  key = {index}
                  className={'${activeIndex === index ? "dot active-dot" : "dot"} '}
                  onClick={() => onClicks(index)} >
                </span>
            ))}
        </div>
    );
}

export default Dots;

### SliderContent.js
javascript
import React from "react";

function SliderContent ({ activeIndex, sliderImage}){
    return(
        <section>
            {sliderImage.map((slide, index) => (
                <div
                    key = {index}
                    className = {index === activeIndex ? "slides active" : "inactive"}>
                    <img className="slide-image" src={slide.urls} alt=""/>
                    <h2 className="slide-title">{slide.title}</h2>
                    <h3 className="slide-text">{slide.description}</h3>
                </div>
            ))}
        </section>
    );
}

export default SliderContent;

### Slider.js
javascript
import first from "../assets/first.jpg";
import second from "../assets/second.jpg";
import third from "../assets/third.jpg";

export default[
    {
        title: "FIRST SLIDE",
        description: "Welcome to the first slide of the carousel",
        urls: first,
    },
    {
        title: "SECOND SLIDE",
        description: "Welcome to the second slide of the carousel",
        urls: second,
    },
    {
        title: "THIRD SLIDE",
        description: "Welcome to the third slide of the carousel",
        urls: third,
    },
];

### Arrows.js
javascript
import React from "react";

function Arrows({prevSlide, nextSlide}){
    return(
        <div className="arrows">
            <span className="prev" onClick={prevSlide}>
                &#10094;
            </span>
            <span className="next" onClick={nextSlide}>
                &#10095;
            </span>
        </div>
    );
}

export default Arrows;

### Dots.js
javascript
import React from "react";

function Dots ({activeIndex, onClicks, sliderImage}){
    return(
        <div className="all-dots">
            {sliderImage.map((slide, index) => (
                <span
                  key = {index}
                  className={'${activeIndex === index ? "dot active-dot" : "dot"} '}
                  onClick={() => onClicks(index)} >
                </span>
            ))}
        </div>
    );
}

export default Dots;

### SliderContent.js
javascript
import React from "react";

function SliderContent ({ activeIndex, sliderImage}){
    return(
        <section>
            {sliderImage.map((slide, index) => (
                <div
                    key = {index}
                    className = {index === activeIndex ? "slides active" : "inactive"}>
                    <img className="slide-image" src={slide.urls} alt=""/>
                    <h2 className="slide-title">{slide.title}</h2>
                    <h3 className="slide-text">{slide.description}</h3>
                </div>
            ))}
        </section>
    );
}

export default SliderContent;

### Slider.js
javascript
import React, {useEffect, useState} from "react";
import SliderContent from "./SliderContent";
import Dots from "./Dots";
import Arrows from "./Arrows";
import SliderImage from "./SliderImage";
import "./slider.css";

const len = SliderImage.length - 1;

function Slider(props){
    const [activeIndex, setActiveIndex] = useState(0);

    useEffect(() => {
        const interval = setInterval (() => {
            setActiveIndex(activeIndex === len ? 0 : activeIndex+1);
        },10000);
        return () => clearInterval(interval);
    }, [activeIndex]);

    return(
        <div className="slider-container">
            <SliderContent activeIndex={activeIndex} sliderImage={SliderImage} />
            <Arrows
                prevSlide={() =>
                    setActiveIndex(activeIndex < 1 ? len : activeIndex - 1)
                } 
                nextSlide={() => 
                    setActiveIndex(activeIndex === len ? 0 : activeIndex + 1)
                }/>
            <Dots
                activeIndex={activeIndex}
                sliderImage={SliderImage}
                onClicks={(activeIndex) => setActiveIndex(activeIndex)}
            />
        </div>
    );
}

export default Slider;

### slider.css
css
* {
    box-sizing: border-box;
    margin: 0;
  }
  
  :root {
    --heights: 50vh;
    --widths: 100%;
  }
  
  .slider-container {
    height: var(--heights);
    width: var(--widths);
    position: relative;
    margin: auto;
    overflow: hidden;
  }
  
  .active {
    display: inline-block;
  }
  
  .inactive {
    display: none;
  }
  
  .slides {
    height: var(--heights);
    width: var(--widths);
    position: relative;
  }
  
  .slide-image {
    width: 100%;
    height: 100%;
    position: absolute;
    object-fit: cover;
  }
  
  .slide-title,
  .slide-text {
    width: 100%;
    height: 100%;
    color: white;
    font-size: 50px;
    position: absolute;
    text-align: center;
    top: 40%;
    z-index: 10;
  }
  
  .slide-text {
    top: 65%;
    font-size: 2rem;
  }
  
  .prev,
  .next {
    color: transparent;
    cursor: pointer;
    z-index: 100;
    position: absolute;
    top: 50%;
    width: auto;
    padding: 1rem;
    margin-top: -3rem;
    font-size: 40px;
    font-weight: bold;
    border-radius: 0px 5px 5px 0px;
  }
  
  .slider-container:hover .prev,.slider-container:hover .next {
    color: black
  }
  
  .slider-container:hover .prev:hover,
  .slider-container:hover .next:hover {
    color: white;
    background-color: rgba(0, 0, 0, 0.6);
    transition: all 0.5s ease-in;
  }
  
  .next {
    right: 0;
    border-radius: 5px 0px 0px 5px;
  }
  
  .all-dots {
    width: 100%;
    height: 100%;
    position: absolute;
    display: flex;
    top: 85%;
    justify-content: center;
    z-index: 200;
  }
  
  .dot {
    cursor: pointer;
    height: 1.5rem;
    width: 1.5rem;
    margin: 0px 3px;
    background-color: transparent;
    /* background-color: rgba(0, 0, 0, 0.3); */
    border-radius: 50%;
    display: inline-block;
  }
  
  .slider-container:hover .dot:hover {
    background-color: rgba(255, 255, 255, 0.5);
  }
  
  /* .active-dot {
    background-color: rgba(255, 255, 255, 0.5);
  } */
  
  .slider-container:hover .dot{
    background-color: rgba(0, 0, 0, 0.3);
  }
  .slider-container:hover .active-dot{
    background-color: rgba(255, 255, 255, 0.5);
  }
  
  .play-pause {
    color: black;
  }

### App.js
javascript
import Slider from "./components/Slider";

function App() {
  return <Slider />;
}

export default App;


## OUTPUT:
### SLIDE 1:
<img width="960" alt="1" src="https://github.com/KeerthikaNagarajan/react-carousel/assets/93427089/5a1f7492-bdc4-4b5d-9043-f1b209b9cfaa">

### SLIDE 2:
<img width="960" alt="2" src="https://github.com/KeerthikaNagarajan/react-carousel/assets/93427089/5d755f25-88b7-415a-a742-542bb7a7127d">

### SLIDE 3:
<img width="960" alt="3" src="https://github.com/KeerthikaNagarajan/react-carousel/assets/93427089/5c09d102-e7bb-47c4-8007-cf6a65b8c3bb">

### RESULT:
The code will generate an image carousel with a set of slides.
