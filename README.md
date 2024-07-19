This JavaScript code sets up an Intersection Observer to track when certain sections of a web page enter or leave the
viewport and updates the class of corresponding links in a sticky sidebar to indicate which section is currently
visible. Here's a detailed breakdown of the code

The **initializeSectionObserver** function is defined without parameters. It will be called immediately after its
definition to set up the observer.

# Selecting Sections
```
const sections = [];
document.querySelectorAll('.right-col-table-content .container > h2').forEach(header => {
const headerContainer = header.closest('.container') ?? null;
if(!headerContainer) return;

headerContainer.dataset.linkId = header.id;
sections.push(headerContainer);
});

```
1. const sections = [];: An empty array is created to store sections that need to be observed.
2. document.querySelectorAll('.right-col-table-content .container > h2'): Selects all h2 elements within containers
   inside an element with the class .right-col-table-content.
3. forEach(header => { ... }): Iterates over each h2 element (header).
4. const headerContainer = header.closest('.container') ?? null;: Finds the closest parent element with the class
   .container for each h2 element. If no such element is found, headerContainer is set to null.
5. if(!headerContainer) return;: If headerContainer is null, the current iteration is skipped.
6. headerContainer.dataset.linkId = header.id;: Sets a data-link-id attribute on the headerContainer element with the
   value of the h2 element's id.
7. sections.push(headerContainer);: Adds the headerContainer to the sections array.

```
const observerOptions = {
root: null, // Use the viewport as the root
rootMargin: '0px',
threshold: 0.3 // Trigger when at least 30% of the section is visible
};

```
1. root: null: The viewport is used as the root element for the observer.
2. rootMargin: '0px': No margin is applied around the root element.
3. threshold: 0.3: The callback is triggered when 30% of the target element is visible in the viewport.

# Defining Observer Callback
```
const observerCallback = (entries, observer) => {
entries.forEach(entry => {
const anchor = document.querySelector(`.sticky-sidebar [href="#${entry.target.dataset.linkId}"]`);
if(!anchor) return;

    if (entry.isIntersecting) {
      anchor.classList.add('active');
    } else {
      anchor.classList.remove('active');
    }

});
};
```

1. entries: An array of IntersectionObserverEntry objects, each representing an observed section.
2. entries.forEach(entry => { ... }): Iterates over each entry.
3. const anchor = document.querySelector(.sticky-sidebar [href="#${entry.target.dataset.linkId}"]);: Finds the
   corresponding anchor element in the sticky sidebar using the data-link-id.
4. if(!anchor) return;: If the anchor is not found, skips the current iteration.
5. if (entry.isIntersecting) { ... } else { ... }: Adds the active class to the anchor if the section is visible (
   isIntersecting is true), otherwise removes the active class.

# Creating and Applying the Observer

const observer = new IntersectionObserver(observerCallback, observerOptions);
sections.forEach(section => observer.observe(section));

1. const observer = new IntersectionObserver(observerCallback, observerOptions);: Creates a new IntersectionObserver
   with the specified callback and options.
2. sections.forEach(section => observer.observe(section));: Starts observing each section stored in the sections array.

# Initializing the Observer

```
initializeSectionObserver();

```
The initializeSectionObserver function is called to set up the observer immediately.