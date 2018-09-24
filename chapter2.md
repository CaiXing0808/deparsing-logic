---
  title: "Building interactive apps with Bokeh"
  description: "Bokeh server applications let you connect all of the powerful Python libraries for analytics and data science, such as NumPy and Pandas, to rich interactive Bokeh visualizations. Learn about Bokeh's built-in widgets,  how to add them to Bokeh documents alongside plots, and how to connect everything to real python code using the Bokeh server."
  attachments: 
    slides_link: "https://s3.amazonaws.com/assets.datacamp.com/production/course_2244/slides/ch4_slides.pdf"
---

## Introducing the Bokeh Server

```yaml
type: VideoExercise 
lang: python
xp: 50 
skills: 2,4
key: 70223a8372 
video_link: //player.vimeo.com/video/154783078 
video_hls: //videos.datacamp.com/transcoded/1392_Bokeh_I/v6/hls-ch4_1.master.m3u8 
```

---

## Understanding Bokeh apps

```yaml
type: PureMultipleChoiceExercise 
lang: python
xp: 50 
skills: 2,4
key: 5f17d13f27   
```


The main purpose of the Bokeh server is to synchronize python objects with web applications in a browser, so that rich, interactive data applications can be connected to powerful PyData libraries such as NumPy, SciPy, Pandas, and scikit-learn. 

What sort of properties can the Bokeh server automatically keep in sync?


`@hint`
Bokeh server is not limited to synchronizing only data sources or glyph properties.

`@possible_answers`
- Only data source objects.
- Only glyph properties.
- [Any property of any Bokeh object.]

`@feedbacks`
- Incorrect. Bokeh server is not limited to synchronizing only data sources.
- Incorrect. Bokeh server is not limited to synchronizing only glyph properties.
- Correct. Bokeh server will automatically keep every property of any Bokeh object in sync.

---

## Using the current document

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: 23b2753b9e   
```


Let's get started with building an interactive Bokeh app. This typically begins with importing the `curdoc`, or "current document", function from
`bokeh.io`. This current document will eventually hold all the plots, controls, and layouts that you create. Your job in this exercise is to use this function to add a single plot to your application.

In the video, Bryan described the process for running a Bokeh app using the `bokeh serve` command line tool. In this chapter and the one that follows, the DataCamp environment does this for you behind the scenes. Notice that your code is part of a `script.py` file. When you hit 'Submit Answer', you'll see in the IPython Shell that we call `bokeh serve script.py` for you. 

Remember, as in the previous chapters, that there are different options available for you to interact with your plots, and as before,
you may have to scroll down to view the lower portion of the plots.


`@instructions`
- Import `curdoc` from `bokeh.io` and `figure` from `bokeh.plotting`.
- Create a new plot called `plot` using the `figure()` function.
- Add a line to the plot using `[1,2,3,4,5]` as the `x` coordinates and `[2,5,4,6,7]` as the `y` coordinates.
- Add the `plot` to the current document using `curdoc().add_root()`. It needs to be passed in as an argument to `add_root()`.

`@hint`
- To import `x` from `y`, you can use the command `from y import x`.
- Use the `figure()` function to create a new plot.
- You can add a line to `plot` using `plot.line()`, where you pass in the list of `x` and `y` coordinates as arguments.
- To add the plot to the current document, `plot` has to be passed in as an argument to `add_root()` in `curdoc().add_root()`.

`@pre_exercise_code`

```{python}
# pec
```

`@sample_code`

```{python}
# Perform necessary imports
from ____ import ____
from ____ import ____

# Create a new plot: plot
plot = ____

# Add a line to the plot
____

# Add the plot to the current document
____
```

`@solution`

```{python}
# Perform necessary imports
from bokeh.plotting import figure
from bokeh.io import curdoc

# Create a new plot: plot
plot = figure()

# Add a line to the plot
plot.line([1,2,3,4,5], [2,5,4,6,7])

# Add the plot to the current document
curdoc().add_root(plot)
```

`@sct`

```{python}
test_import('bokeh.plotting.figure',
            not_imported_msg='Did you import `figure` from `bokeh.plotting`?',
            incorrect_as_msg='Did you import `figure` from `bokeh.plotting`?')
                 
test_import('bokeh.plotting.figure',
            not_imported_msg='Did you import `curdoc` from `bokeh.io`?',
            incorrect_as_msg='Did you import `curdoc` from `bokeh.io`?')

test_function("bokeh.plotting.figure")

sig = sig_from_params(param("x", param.POSITIONAL_OR_KEYWORD),
                      param("y", param.POSITIONAL_OR_KEYWORD, default=0),
                      param("kwargs", param.VAR_KEYWORD))
                      
test_function_v2("plot.line", params=['x'], signature=sig, incorrect_msg= 'Did you pass the correct x-axis data to `plot.line()`?')
test_function_v2("plot.line", params=['y'], signature=sig, incorrect_msg='Did you pass the correct y-axis data to `plot.line()`?')


test_function("bokeh.io.curdoc.add_root", do_eval=False)

success_msg("Great work! In the next exercise, you'll practice adding a single slider to a current document.")
```

---

## Add a single slider

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: b5ac403fd2   
```


In the previous exercise, you added a single plot to the "current document" of your application. In this exercise, you'll practice adding a layout to your current document. 

Your job here is to create a single slider, use it to create a widgetbox layout, and then add this layout to the current document. 

The slider you create here cannot be used for much, but in the later exercises, you'll use it to update your plots!


`@instructions`
- Import `curdoc` from `bokeh.io`, `widgetbox` from `bokeh.layouts`, and `Slider` from `bokeh.models`.
- Create a slider called `slider` by using the `Slider()` function and specifying the parameters `title`, `start`, `end`, `step`, and `value`.
- Use the slider to create a widgetbox layout called `layout`.
- Add the layout to the current document using `curdoc().add_root()`. It needs to be passed in as an argument to `add_root()`.

`@hint`
- You can import `x` from `y` using the command `from y import x`.
- You can create a slider using the `Slider()` function and keyword arguments `title='my slider'`, `start=0`, `end=10`, `step=0.1`, `value=2`.
- To create a `widgetbox` layout, pass `slider` as an argument to `widgetbox()`.
- To add the layout to the current document, `layout` has to be passed in as an argument to `add_root()` in `curdoc().add_root()`.

`@pre_exercise_code`

```{python}
# pec
```

`@sample_code`

```{python}
# Perform the necessary imports
from ____ import ____
from ____ import ____
from ____ import ____

# Create a slider: slider
slider = ____(____='my slider', ____=0, ____=10, ____=0.1, ____=2)

# Create a widgetbox layout: layout
layout = ____

# Add the layout to the current document
____
```

`@solution`

```{python}
# Perform the necessary imports
from bokeh.io import curdoc
from bokeh.layouts import widgetbox
from bokeh.models import Slider

# Create a slider: slider
slider = Slider(title='my slider', start=0, end=10, step=0.1, value=2)

# Create a widgetbox layout: layout
layout = widgetbox(slider)

# Add the layout to the current document
curdoc().add_root(layout)
```

`@sct`

```{python}
test_import('bokeh.io.curdoc',
            not_imported_msg='Did you import `curdoc` from `bokeh.io`?',
            incorrect_as_msg='Did you import `curdoc` from `bokeh.io`?')
            
test_import('bokeh.layouts.widgetbox',
            not_imported_msg='Did you import `widgetbox` from `bokeh.layouts`?',
            incorrect_as_msg='Did you import `widgetbox` from `bokeh.layouts`?')
            
test_import('bokeh.models.Slider',
            not_imported_msg='Did you import `Slider` from `bokeh.models`?',
            incorrect_as_msg='Did you import `Slider` from `bokeh.models`?')

test_function("bokeh.models.Slider")

test_function("bokeh.layouts.widgetbox", do_eval=False)

test_function("bokeh.io.curdoc.add_root", do_eval=False)

success_msg("Good job! You'll build on this in the next exercise by adding another slider to the current document!")
```

---

## Multiple sliders in one document

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: 96f949ebfb   
```


Having added a single slider in a widgetbox layout to your current document, you'll now add multiple sliders into the current document.

Your job in this exercise is to create two sliders, add them to a widgetbox layout, and then add the layout into the current document.


`@instructions`
- Create the first slider, `slider1`, using the `Slider()` function. Give it a title of `'slider1'`. Have it `start` at `0`, `end` at `10`, with a `step` of `0.1` and initial `value` of `2`.
- Create the second slider, `slider2`, using the `Slider()` function. Give it a title of `'slider2'`. Have it `start` at `10`, `end` at `100`, with a `step` of `1` and initial `value` of `20`.
- Use `slider1` and `slider2` to create a widgetbox layout called `layout`.
- Add the layout to the current document using `curdoc().add_root()`. This has already been done for you.

`@hint`
- You can create the first slider using the `Slider()` function with keyword arguments `title='slider1'`, `start=0`, `end=10`, `step=0.1`, `value=2`.
- You can create the second slider using the `Slider()` function with keyword arguments `title='slider2'`, `start=10`, `end=100`, `step=1`, `value=20.`
- To create a `widgetbox` layout, pass `slider1` and `slider2` as arguments to `widgetbox()`.
- The layout has already been added to the current document. So hit 'Submit Answer' to view the two sliders!

`@pre_exercise_code`

```{python}
# pec
```

`@sample_code`

```{python}
# Perform necessary imports
from bokeh.io import curdoc
from bokeh.layouts import widgetbox
from bokeh.models import Slider

# Create first slider: slider1
slider1 = ____

# Create second slider: slider2
slider2 = ____

# Add slider1 and slider2 to a widgetbox
layout = ____

# Add the layout to the current document
curdoc().add_root(layout)
```

`@solution`

```{python}
# Perform necessary imports
from bokeh.io import curdoc
from bokeh.layouts import widgetbox
from bokeh.models import Slider

# Create first slider: slider1
slider1 = Slider(title='slider1', start=0, end=10, step=0.1, value=2)

# Create second slider: slider2
slider2 = Slider(title='slider2', start=10, end=100, step=1, value=20)

# Add slider1 and slider2 to a widgetbox
layout = widgetbox(slider1, slider2)

# Add the layout to the current document
curdoc().add_root(layout)
```

`@sct`

```{python}
test_object("slider1", do_eval=False)
test_function("bokeh.models.Slider", index=1)

test_object("slider2", do_eval=False)
test_function("bokeh.models.Slider", index=2)

test_object("layout", do_eval=False)


test_function("bokeh.layouts.widgetbox", do_eval=False)

# Test: Predefined Code
predef_msg = "You don't have to change any of the predefined code."

test_import(
    "bokeh.io.curdoc",
    same_as = True,
    not_imported_msg = predef_msg,
    incorrect_as_msg = predef_msg
)
test_import(
    "bokeh.layouts.widgetbox",
    same_as = True,
    not_imported_msg = predef_msg,
    incorrect_as_msg = predef_msg
)
test_import(
    "bokeh.models.Slider",
    same_as = True,
    not_imported_msg = predef_msg,
    incorrect_as_msg = predef_msg
)


test_function("bokeh.io.curdoc.add_root", do_eval=False)

success_msg("Excellent! The next step now is to learn how to connect these sliders to plots.")
```

---

## Connecting sliders to plots

```yaml
type: VideoExercise 
lang: python
xp: 50 
skills: 2,4
key: 022c052521 
video_link: //player.vimeo.com/video/154783078 
video_hls: //videos.datacamp.com/transcoded/1392_Bokeh_I/v3/hls-ch4_2.master.m3u8 
```

---

## Adding callbacks to sliders

```yaml
type: PureMultipleChoiceExercise 
lang: python
xp: 50 
skills: 2,4
key: e466b9e3f4   
```


Callbacks are functions that a user can define, like `def callback(attr, old, new)`, that can be called automatically when some property of a Bokeh object (e.g., the `value` of a `Slider`) changes. 

How are callbacks added for the `value` property of `Slider` objects?


`@hint`
A `Slider` does not have a `callback` method or an `update` property.

`@possible_answers`
- By passing a callback function to the `callback` method.
- [By passing a callback function to the `on_change` method.]
- By assigning the callback function to the `Slider.update` property.

`@feedbacks`
- Incorrect. `Slider` does not have a `callback` method.
- Correct. A callback is added by calling `myslider.on_change('value', callback)`.
- Incorrect. `Slider` does not have an `update` property.

---

## How to combine Bokeh models into layouts

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: 38503d044b   
```


Let's begin making a Bokeh application that has a simple slider and plot, that also updates the plot based on the slider. 

In this exercise, your job is to first explicitly create a ColumnDataSource. You'll then combine a plot and a slider into a single column layout, and add it to the current document. 

After you are done, notice how in the figure you generate, the slider will not actually update the plot, because a widget callback has not been defined. You'll learn how to update the plot using widget callbacks in the next exercise.

All the necessary modules have been imported for you. The plot is available in the workspace as `plot`, and the slider is available as `slider`.


`@instructions`
- Create a ColumnDataSource called `source`. Explicitly specify the `data` parameter of `ColumnDataSource()` with `{'x': x, 'y': y}`.
- Add a line to the figure `plot`, with `'x'` and `'y'` from the ColumnDataSource.
- Combine the slider and the plot into a column layout called `layout`. Be sure to first create a widgetbox layout using `widgetbox()` with `slider` and pass that into the `column()` function along with `plot`.

`@hint`
- You can create the ColumnDataSource using the `ColumnDataSource()` function and specifying the keyword argument `data={'x': x, 'y': y}`.
- Inside `plot.line()`, pass in `'x'` and `'y'` from the ColumnDataSource object. Be sure to also specify `source=source`.
- You can create the column layout using the `column()` function and passing in the arguments `widgetbox(slider)` and `plot`.

`@pre_exercise_code`

```{python}
# pec
import numpy as np
from bokeh.io import curdoc
from bokeh.plotting import figure

x = np.linspace(0.3, 10, 300)
y = np.sin(1/x)

from bokeh.layouts import widgetbox, column
from bokeh.models import ColumnDataSource, Slider

plot = figure()
slider = Slider(title="scale", start=1, end=10, step=1, value=1)
```

`@sample_code`

```{python}
# Create ColumnDataSource: source
source = ____

# Add a line to the plot
plot.line(____, ____, source=____)

# Create a column layout: layout
layout = ____(____, ____)

# Add the layout to the current document
curdoc().add_root(layout)
```

`@solution`

```{python}
# Create ColumnDataSource: source
source = ColumnDataSource(data={'x': x, 'y': y})

# Add a line to the plot
plot.line('x', 'y', source=source)

# Create a column layout: layout
layout = column(widgetbox(slider), plot)

# Add the layout to the current document
curdoc().add_root(layout)
```

`@sct`

```{python}
test_object("source", do_eval=False)
test_function("bokeh.models.ColumnDataSource")

def my_converter0(source):
    return dict(source.data)
    
set_converter(key = "bokeh.models.sources.ColumnDataSource", fundef = my_converter0)

test_function("plot.line" , args=[], keywords=['source'] , index=1, incorrect_msg='In your call to `plot.line()`, did you pass `source` as argument to the keyword `source`?')

sig = sig_from_params(param("x", param.POSITIONAL_OR_KEYWORD),
                      param("y", param.POSITIONAL_OR_KEYWORD, default=0),
                      param("kwargs", param.VAR_KEYWORD))
                      
test_function_v2("plot.line", params=['x'], signature=sig, incorrect_msg= 'Did you pass the correct x-axis data to `plot.line()`?')
test_function_v2("plot.line", params=['y'], signature=sig, incorrect_msg='Did you pass the correct y-axis data to `plot.line()`?')



test_function("bokeh.layouts.widgetbox", do_eval=False)

test_function("bokeh.layouts.column", do_eval=False)

test_object("layout", do_eval=False)

# Test: Predefined Code
predef_msg = "You don't have to change any of the predefined code."

test_function("bokeh.io.curdoc.add_root", do_eval=False)

success_msg("Great work! Since a widget callback hasn't been defined here, the slider does not update the figure.")
```

---

## Learn about widget callbacks

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: 645d4dfbaf   
```


You'll now learn how to use widget callbacks to update the state of a Bokeh application, and in turn, the data that is presented to the user. 

Your job in this exercise is to use the slider's `on_change()` function to update the plot's data from the previous example. NumPy's `sin()` function will be used to update the y-axis data of the plot.

Now that you have added a widget callback, notice how as you move the slider of your app, the figure also updates!


`@instructions`
- Define a callback function `callback` with the parameters `attr`, `old`, `new`.
- Read the current value of `slider` as a variable `scale`. You can do this using `slider.value`.
- Compute values for the updated y using `np.sin(scale/x)`.
- Update `source.data` with the new data dictionary.
- Attach the callback to the `'value'` property of `slider`.  This can be done using `on_change()` and passing in `'value'` and `callback`.

`@hint`
- To define a function, use the `def` keyword followed by the name of the function. In parantheses, specify the function's parameters.
- To read the current value of slider, use `slider.value`. Assign the result of this to `scale`.
- Compute the new y values using `np.sin(scale/x)`.
- To update `source.data`, pass in the new values for `x` and `y`. The value for `x` remains the same, but use `new_y` to update `y`.
- To attach the callback, you can use `slider.on_change()` with the arguments `'value'` and `callback`.

`@pre_exercise_code`

```{python}
# pec
import numpy as np
from bokeh.plotting import figure, curdoc
from bokeh.layouts import widgetbox, column
from bokeh.models import ColumnDataSource, Slider

slider = Slider(title="scale", start=1, end=10, step=1, value=1)

x = np.linspace(0.3, 10, 300)
y = np.sin(1/x)
source = ColumnDataSource(data={'x': x, 'y': y})

plot = figure()
plot.line('x', 'y', source=source)
```

`@sample_code`

```{python}
# Define a callback function: callback
____ ____(____, ____, ____):

    # Read the current value of the slider: scale
    scale = ____

    # Compute the updated y using np.sin(scale/x): new_y
    new_y = ____

    # Update source with the new data values
    source.data = {'x': ____, 'y': ____}

# Attach the callback to the 'value' property of slider
____

# Create layout and add to current document
layout = column(widgetbox(slider), plot)
curdoc().add_root(layout)
```

`@solution`

```{python}
# Define a callback function: callback
def callback(attr, old, new):

    # Read the current value of the slider: scale
    scale = slider.value

    # Compute the updated y using np.sin(scale/x): new_y
    new_y = np.sin(scale/x)

    # Update source with the new data values
    source.data = {'x': x, 'y': new_y}

# Attach the callback to the 'value' property of slider
slider.on_change('value', callback)

# Create layout and add to current document
layout = column(widgetbox(slider), plot)
curdoc().add_root(layout)
```

`@sct`

```{python}
# MC-note: converter appears to be working as expected
def my_converter(source):
    return dict(source.data)

set_converter(key = "bokeh.models.sources.ColumnDataSource", fundef = my_converter)


pre_code = """
slider = Slider(title="scale", start=1, end=10, step=1, value=4)
x = np.linspace(0.3, 10, 300)
y = np.sin(1/x)
source = ColumnDataSource(data={'x': x, 'y': y})
"""

def inner_test():
    test_object_after_expression("scale")
    test_object_after_expression("new_y",  
    undefined_msg="Did you define `new_y`?",
    incorrect_msg="Did you assign the correct value to `new_y`?")
    test_object_after_expression("source", pre_code=pre_code, incorrect_msg="Are you sure you assigned the correct value to `source.data`? If moving the slider isn't updating the values of the plot as it should, take another look at how you updated `source.data`.")

test_function_definition("callback", body=inner_test)

# Test: Predefined Code
predef_msg = "You don't have to change any of the predefined code."

test_function("slider.on_change", do_eval=False)

test_object("layout", do_eval=False,
            undefined_msg = predef_msg,
            incorrect_msg = predef_msg
)


test_function("bokeh.layouts.widgetbox", do_eval=False,
                 not_called_msg = predef_msg,
                 incorrect_msg = predef_msg
)


test_function("bokeh.layouts.column", do_eval=False)

test_function("bokeh.plotting.curdoc.add_root", do_eval=False)

success_msg("Fantastic! Now that you have added the callback, notice how the figure updates along with the slider!")
```

---

## Updating plots from dropdowns

```yaml
type: VideoExercise 
lang: python
xp: 50 
skills: 2,4
key: 3ba1cc526c 
video_link: //player.vimeo.com/video/154783078 
video_hls: //videos.datacamp.com/transcoded/1392_Bokeh_I/v3/hls-ch4_3.master.m3u8 
```

---

## Updating data sources from dropdown callbacks

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: 53952c3d79   
```


You'll now learn to update the plot's data using a drop down menu instead of a slider. This would allow users to do things like select between different data sources to view. 

The ColumnDataSource `source` has been created for you along with the plot.
Your job in this exercise is to add a drop down menu to update the plot's data.

All necessary modules have been imported for you.


`@instructions`
- Define a callback function called `update_plot` with the parameters `attr`, `old`, `new`.
    - If the `new` selection is `female_literacy`, update the `y` value of the ColumnDataSource to `female_literacy`. Else, `y` should be `population`. 
    - `x` remains `fertility` in both cases.
- Create a dropdown select widget using `Select()`. Specify the parameters `title`, `options`, and `value`. The `options` are `'female_literacy'` and `'population'`, while the `value` is `'female_literacy'`.
- Attach the callback to the `'value'` property of `select`.  This can be done using `on_change()` and passing in `'value'` and `update_plot`.

`@hint`
- In both conditions of the if/else statement, the `x` data of `source` is `fertility`. The `y` value becomes `female_literacy` if the `new` selection is `'female_literacy'`. Otherwise, it should be `population`.
- You can create the dropdown select widget called `select` using the `Select()` function and specifying the `title`, `'female_literacy'` and `'population'` as the `options`, and `'female_literacy'` as the `value`.
- To attach the callback, you can use `select.on_change()` with the arguments `'value'` and `update_plot`.

`@pre_exercise_code`

```{python}
# pec
import pandas as pd
df = pd.read_csv('https://s3.amazonaws.com/assets.datacamp.com/production/course_1392/datasets/literacy_birth_rate.csv')
df = df.dropna()
df['population'] /= 1000000

female_literacy = df['female literacy']
population = df['population']
fertility = df['fertility']

# Import figure from bokeh.plotting
from bokeh.plotting import figure

# Import output_file and show from bokeh.io
from bokeh.io import curdoc
from bokeh.models import  ColumnDataSource
from bokeh.layouts import row
```

`@sample_code`

```{python}
# Perform necessary imports
from bokeh.models import ColumnDataSource, Select

# Create ColumnDataSource: source
source = ColumnDataSource(data={
    'x' : fertility,
    'y' : female_literacy
})

# Create a new plot: plot
plot = figure()

# Add circles to the plot
plot.circle('x', 'y', source=source)

# Define a callback function: update_plot
def update_plot(attr, old, new):
    # If the new Selection is 'female_literacy', update 'y' to female_literacy
    if new == ____: 
        source.data = {
            'x' : ____,
            'y' : ____
        }
    # Else, update 'y' to population
    else:
        source.data = {
            'x' : ____,
            'y' : ____
        }

# Create a dropdown Select widget: select    
select = ____(____="distribution", options=[____, ____], ____=____)

# Attach the update_plot callback to the 'value' property of select
select.____(____, ____)

# Create layout and add to current document
layout = row(select, plot)
curdoc().add_root(layout)
```

`@solution`

```{python}
# Perform necessary imports
from bokeh.models import ColumnDataSource, Select

# Create ColumnDataSource: source
source = ColumnDataSource(data={
    'x' : fertility,
    'y' : female_literacy
})

# Create a new plot: plot
plot = figure()

# Add circles to the plot
plot.circle('x', 'y', source=source)

# Define a callback function: update_plot
def update_plot(attr, old, new):
    # If the new Selection is 'female_literacy', update 'y' to female_literacy
    if new == 'female_literacy': 
        source.data = {
            'x' : fertility,
            'y' : female_literacy
        }
    # Else, update 'y' to population
    else:
        source.data = {
            'x' : fertility,
            'y' : population
        }

# Create a dropdown Select widget: select
select = Select(title="distribution", options=["female_literacy", "population"], value='female_literacy')

# Attach the update_plot callback to the 'value' property of select
select.on_change('value', update_plot)

# Create layout and add to current document
layout = row(select, plot)
curdoc().add_root(layout)
```

`@sct`

```{python}
import copy
test_function("bokeh.models.ColumnDataSource", do_eval=False)


test_object("source")

test_object("plot")
test_function("bokeh.plotting.figure")

# MC-note: was likely an error in the converter. pythonwhat was accidentally passing submissions
#          when converters threw errors, but this has been fixed and will be pushed to prod soon.
def my_converter0(source):
    return {k: list(v.values) for k,v in source.data.items()}
    
set_converter(key = "bokeh.models.sources.ColumnDataSource", fundef = my_converter0)

test_function("plot.circle" , args=[], keywords=['source'] , index=1, incorrect_msg='In your call to `plot.circle()`, did you pass `source` as argument to the keyword `source`?', do_eval=False)

sig = sig_from_params(param("x", param.POSITIONAL_OR_KEYWORD),
                      param("y", param.POSITIONAL_OR_KEYWORD, default=0),
                      param("kwargs", param.VAR_KEYWORD))
                      
test_function_v2("plot.circle", params=['x'], signature=sig, incorrect_msg= 'Did you pass the correct x-axis data to `plot.circle()`?')
test_function_v2("plot.circle", params=['y'], signature=sig, incorrect_msg='Did you pass the correct y-axis data to `plot.circle()`?')

#test_function("plot.circle", index=1, do_eval=False)

new_source = """source = ColumnDataSource(data={})"""

# MC-note: appears resolved after fixing converter
#def inner_test():
    #test_object_after_expression("source", extra_env={'new' : 'female_literacy'})
    #test_object_after_expression("source", extra_env={'new' : 'population'})
def inner_test():
    test_if_else(
            test = test_expression_result(extra_env={'new' : 'female_literacy'}, incorrect_msg="Did you test whether `'new'` is equal to `'female_literacy'`?"),
            body = test_object_after_expression('source', pre_code=new_source, incorrect_msg="Are you sure you assigned the correct value to `source.data`?"),
            orelse = test_object_after_expression('source', pre_code=new_source, incorrect_msg="Are you sure you assigned the correct value to `source.data`?")
            )
test_function_definition("update_plot", body=inner_test)

test_object("select", do_eval=False)
test_function("bokeh.models.Select")

# Test: Predefined Code
predef_msg = "You don't have to change any of the predefined code."

test_object("layout", do_eval=False,
            undefined_msg = predef_msg,
            incorrect_msg = predef_msg
)

test_function("bokeh.layouts.row", do_eval=False,
                 not_called_msg = predef_msg,
                 incorrect_msg = predef_msg
)


test_function("bokeh.io.curdoc.add_root", do_eval=False,
                 not_called_msg = predef_msg,
                 incorrect_msg = predef_msg
)

def my_converter1(x):
        return type(x)
        
set_converter(key = "bokeh.plotting.figure.Figure", fundef = my_converter1)

success_msg("Great work! Dropdowns can add a lot of interactivity to figures. In the next exercise, you'll learn how to synchronize two dropdowns.")
```

---

## Synchronize two dropdowns

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: 069eeeafe2   
```


Here, you'll practice using a dropdown callback to update another dropdown's options. This will allow you to customize your applications even further and is a powerful addition to your toolbox. 

Your job in this exercise is to create two dropdown select widgets and then define a callback such that one dropdown is used to update the other dropdown.

All modules necessary have been imported.


`@instructions`
- Create `select1`, the first dropdown select widget. Specify the parameters `title`, `options`, and `value`.
- Create `select2`, the second dropdown select widget. Specify the parameters `title`, `options`, and `value`.
- Inside the callback function, if `select1` equals `'A'`, update the options of `select2` to `['1', '2', '3']` and set its value to `'1'`.
- If `select1` does not equal `'A'`, update the options of `select2` to `['100', '200', '300']` and set its value to `'100'`.
- Attach the callback to the `'value'` property of `select1`.  This can be done using `on_change()` and passing in `'value'` and `callback`.

`@hint`
- You can create the dropdown select widgets using the `Select()` function and specifying the `title`, `options`, and `value` parameters.
- If `select1 == 'A'`, set `select2.options` to `['1', '2', '3']` and `select2.value` to `'1'`.
- Else, set `select2.options` to `['100', '200', '300']` and `select2.value` to `'100'`.
- To attach the callback, you can use `select1.on_change()` with the arguments `'value'` and `callback`.

`@pre_exercise_code`

```{python}
# Perform necessary imports
from bokeh.plotting import figure, curdoc
from bokeh.layouts import widgetbox, column
from bokeh.models import ColumnDataSource, Select
```

`@sample_code`

```{python}
# Create two dropdown Select widgets: select1, select2
select1 = Select(____='First', ____=['A', 'B'], ____='A')
select2 = Select(____='Second', ____=['1', '2', '3'], ____='1')

# Define a callback function: callback
def callback(attr, old, new):
    # If select1 is 'A' 
    if ____ == ____:
        # Set select2 options to ['1', '2', '3']
        select2.options = ____

        # Set select2 value to '1'
        select2.value = ____
    else:
        # Set select2 options to ['100', '200', '300']
        ____ = ____

        # Set select2 value to '100'
        ____ = ____

# Attach the callback to the 'value' property of select1
select1.____(____, ____)

# Create layout and add to current document
layout = widgetbox(select1, select2)
curdoc().add_root(layout)
```

`@solution`

```{python}
# Create two dropdown Select widgets: select1, select2
select1 = Select(title='First', options=['A', 'B'], value='A')
select2 = Select(title='Second', options=['1', '2', '3'], value='1')

# Define a callback function: callback
def callback(attr, old, new):
    # If select1 is 'A' 
    if select1.value == 'A':
        # Set select2 options to ['1', '2', '3']
        select2.options = ['1', '2', '3']
        
        # Set select2 value to '1'
        select2.value = '1'
    else:
        # Set select2 options to ['100', '200', '300']
        select2.options = ['100', '200', '300']
        
        # Set select2 value to '100'
        select2.value = '100'

# Attach the callback to the 'value' property of select1
select1.on_change('value', callback)

# Create layout and add to current document
layout = widgetbox(select1, select2)
curdoc().add_root(layout)
```

`@sct`

```{python}
test_object("select1", do_eval=False)
test_function("bokeh.models.Select")

test_object("select2", do_eval=False)
test_function("bokeh.models.Select", index=1)
test_function("bokeh.models.Select", index=2)
test_function("select1.on_change", do_eval=False)



# MC-note: select.options was an unpickle-able bokeh class that caused the session to hang :(
#          will open an issue to get pw to raise an informative error message
def conv_select(select):
    return [select.title, list(select.options), select.value]

set_converter('bokeh.models.widgets.inputs.Select', conv_select)


# MC-note: I can't think of an easy way to test specific attributes of select2 :(
#          on the upshot, I know exactly how to respec these functions now...
#          test_expression_result won't work because it won't run the code it needs to AND expr_code="select2.title", etc..

pre_code = """
select1 = Select(title="First", options=["A", "B"], value="A")
select2 = Select(title="Second", options=["1", "2", "3"], value="1")
"""
def inner_test():
    test_if_else(
            test = test_expression_result(context_vals=["A"], incorrect_msg= "Did you check whether `select1.value` is equal to `\"A\"`?"),
            body = test_object_after_expression('select2', incorrect_msg="Did you assign the correct list to `select2.options` and the correct value to `select2.value`?", pre_code = pre_code),
            orelse = test_object_after_expression('select2', incorrect_msg="Did you assign the correct list to `select2.options` and the correct value to `select2.value`?", pre_code = pre_code)
            )

test_function_definition("callback", body=inner_test)

# Test: Predefined Code
predef_msg = "You don't have to change any of the predefined code."

test_object("layout", do_eval=False,
            undefined_msg = predef_msg,
            incorrect_msg = predef_msg
)

test_function("bokeh.layouts.widgetbox", do_eval=False,
                 not_called_msg = predef_msg,
                 incorrect_msg = predef_msg
)



test_function("bokeh.plotting.curdoc.add_root", do_eval=False,
                 not_called_msg = predef_msg,
                 incorrect_msg = predef_msg
)

success_msg("Great work! Play around by flipping the first dropdown option between A and B and observe how doing that changes the value of the second dropdown.")
```

---

## Buttons

```yaml
type: VideoExercise 
lang: python
xp: 50 
skills: 2,4
key: 913779f5ef 
video_link: //player.vimeo.com/video/154783078 
video_hls: //videos.datacamp.com/transcoded/1392_Bokeh_I/v3/hls-ch4_4.master.m3u8 
```

---

## Button widgets

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: 1d36760131   
```


It's time to practice adding buttons to your interactive visualizations. Your job in this exercise is to create a button and use its `on_click()` method to update a plot. 

All necessary modules have been imported for you. In addition, the ColumnDataSource with data `x` and `y` as well as the figure have been created for you and are available in the workspace as `source` and `plot`.

When you're done, be sure to interact with the button you just added to your plot, and notice how it updates the data!


`@instructions`
- Create a button called `button` using the function `Button()` with the label `'Update Data'`.
- Define an update callback `update()` with no arguments.
- Compute new y values using the code provided.
- Update the ColumnDataSource data dictionary `source.data` with the new y value.
- Add the update callback to the button using `on_click()`.

`@hint`
- You can create a button using the `Button()` function and specifying the keyword argument `label='Update Data'`.
- Use the keyword `def` to define a function called `update()`. It takes in no arguments.
- Assign the new y values computed using `np.sin(x) + np.random.random(N)` to `y`.
- Update `source.data` with the new `y` value. `x` remains the same.
- You can add the update callback to the button using `button.on_click()` and passing in the `update` callback.

`@pre_exercise_code`

```{python}
# pec
import numpy as np
from bokeh.plotting import figure, curdoc
from bokeh.layouts import widgetbox, column
from bokeh.models import ColumnDataSource, Button

N = 200
x = np.linspace(0, 10, N)
y = np.sin(x) + np.random.random(N)

# This code is written
source = ColumnDataSource(data={'x': x, 'y': y})
plot = figure()
plot.circle('x', 'y', source=source)
```

`@sample_code`

```{python}
# Create a Button with label 'Update Data'
button = ____

# Define an update callback with no arguments: update
____ ____:

    # Compute new y values: y
    ____ = np.sin(x) + np.random.random(N)

    # Update the ColumnDataSource data dictionary
    ____ = ____

# Add the update callback to the button
____

# Create layout and add to current document
layout = column(widgetbox(button), plot)
curdoc().add_root(layout)
```

`@solution`

```{python}
# Create a Button with label 'Update Data'
button = Button(label='Update Data')

# Define an update callback with no arguments: update
def update():

    # Compute new y values: y
    y = np.sin(x) + np.random.random(N)

    # Update the ColumnDataSource data dictionary
    source.data = {'x': x, 'y': y}

# Add the update callback to the button
button.on_click(update)

# Create layout and add to current document
layout = column(widgetbox(button), plot)
curdoc().add_root(layout)
```

`@sct`

```{python}
# Set converter so that ColumnDataSource can be fetched from the process properly
set_converter(key = "bokeh.models.sources.ColumnDataSource", fundef = lambda source: dict(source.data))   

Ex().check_function("bokeh.models.Button")
Ex().check_object("button")

Ex().check_function_def('update').check_body().multi(
    F().test_correct(
        has_equal_value(name='y', pre_code='np.random.seed(42)',
                        error_msg="There's an error. Have you used `{'x': x, 'y': y}` to specify `source.data`?"),
        multi(
            check_function('numpy.sin', signature=False).check_args(0).has_equal_ast(),
            check_function('numpy.random.random', signature=False).check_args(0).has_equal_ast()
        )
    ),
    has_equal_value(name='source', pre_code='np.random.seed(42)',
                    incorrect_msg="Have you used `{'x': x, 'y': y}` to specify `source.data`?")    
)

Ex().check_function("button.on_click").check_args(0).has_equal_ast()


predef_msg = "Please keep the code that sets up the layout and adds it to the current document."
Ex().check_object('layout', missing_msg=predef_msg)
Ex().check_function('bokeh.layouts.column', missing_msg=predef_msg, signature=False)
Ex().check_function('bokeh.layouts.widgetbox', missing_msg=predef_msg, signature=False)
Ex().check_function('bokeh.plotting.curdoc.add_root', missing_msg=predef_msg, signature=False)

success_msg("Great work! Notice how clicking the button updates the data in accordance with the sinusoidal function you wrote.")
```

---

## Button styles

```yaml
type: BokehServerExercise 
lang: python
xp: 100 
skills: 2,4
key: db5dce90b0   
```


You can also get really creative with your `Button` widgets. 

In this exercise, you'll practice using CheckboxGroup, RadioGroup, and Toggle to add multiple `Button` widgets with different styles.

`curdoc` and `widgetbox` have already been imported for you.


`@instructions`
- Import `CheckboxGroup`, `RadioGroup`, `Toggle` from `bokeh.models`.
- Add a Toggle called `toggle` using the `Toggle()` function with `button_type` `'success'` and `label` `'Toggle button'`.
- Add a CheckboxGroup called `checkbox` using the `CheckboxGroup()` function with `labels=['Option 1', 'Option 2', 'Option 3']`.
- Add a RadioGroup called `radio` using the `RadioGroup()` function with `labels=['Option 1', 'Option 2', 'Option 3']`.
- Add a widgetbox containing the Toggle `toggle`, CheckboxGroup `checkbox`, and RadioGroup `radio` to the current document.

`@hint`
- You can use the command `from y import x` to import `x` from `y`.
- You can add the toggle use the `Toggle()` function and specifying the keyword arguments `label='Toggle button'` and `button_type='success'`.
- You can add the CheckboxGroup using the `CheckboxGroup()` function and specifying the keyword argument `labels=['Option 1', 'Option 2', 'Option 3']`.
- You can add the RadioGroup using the `RadioGroup()` function and specifying the keyword argument `labels=['Option 1', 'Option 2', 'Option 3']`.
- To add the widgetbox layout to the current document, `widgetbox(toggle, checkbox, radio)` has to be passed in as an argument to `add_root()` in `curdoc().add_root()`.

`@pre_exercise_code`

```{python}
# pec

from bokeh.io import curdoc
from bokeh.layouts import widgetbox
```

`@sample_code`

```{python}
# Import CheckboxGroup, RadioGroup, Toggle from bokeh.models


# Add a Toggle: toggle
toggle = ____

# Add a CheckboxGroup: checkbox
checkbox = ____

# Add a RadioGroup: radio
radio = ____

# Add widgetbox(toggle, checkbox, radio) to the current document
curdoc().add_root(____)
```

`@solution`

```{python}
# Import CheckboxGroup, RadioGroup, Toggle from bokeh.models
from bokeh.models import CheckboxGroup, RadioGroup, Toggle

# Add a Toggle: toggle
toggle = Toggle(label='Toggle button', button_type='success')

# Add a CheckboxGroup: checkbox
checkbox = CheckboxGroup(labels=['Option 1', 'Option 2', 'Option 3'])

# Add a RadioGroup: radio
radio = RadioGroup(labels=['Option 1', 'Option 2', 'Option 3'])

# Add widgetbox(toggle, checkbox, radio) to the current document
curdoc().add_root(widgetbox(toggle, checkbox, radio))
```

`@sct`

```{python}
test_import('bokeh.models.CheckboxGroup',
            not_imported_msg='Did you import `CheckboxGroup` from `bokeh.models`?',
            incorrect_as_msg='Did you import `CheckboxGroup` from `bokeh.models`?')
            
test_import('bokeh.models.RadioGroup',
            not_imported_msg='Did you import `RadioGroup` from `bokeh.models`?',
            incorrect_as_msg='Did you import `RadioGroup` from `bokeh.models`?')
            
test_import('bokeh.models.Toggle',
            not_imported_msg='Did you import `Toggle` from `bokeh.models`?',
            incorrect_as_msg='Did you import `Toggle` from `bokeh.models`?')

test_object("toggle", do_eval=False)

test_function("bokeh.models.Toggle")

test_object("checkbox", do_eval=False)

test_function("bokeh.models.CheckboxGroup")

test_object("radio", do_eval=False)

test_function("bokeh.models.RadioGroup")

# Test: Predefined Code
predef_msg = "You don't have to change any of the predefined code."

test_function("bokeh.layouts.widgetbox", do_eval=False,
                 not_called_msg = predef_msg,
                 incorrect_msg = predef_msg
)


test_function("bokeh.io.curdoc.add_root", do_eval=False,
                 not_called_msg = predef_msg,
                 incorrect_msg = predef_msg
)

success_msg("Great work! Toggles, CheckboxGroups, and RadioGroups allow you to add your own stylistic touch to buttons in Bokeh apps.")
```

---

## Hosting applications for wider audiences

```yaml
type: VideoExercise 
lang: python
xp: 50 
skills: 2,4
key: 5d54d699df 
video_link: //player.vimeo.com/video/154783078 
video_hls: //videos.datacamp.com/transcoded/1392_Bokeh_I/v6/hls-ch4_5.master.m3u8 
```
