# Keras Image classification walkthrough

I have struggled for some time with image classification - there seems to be so many ways to accomplish this, the framework was for ever changing.

So - time to get this sorted.


# Basic Shape Identification

We will try and Identify 3 shapes

  - Circle
  - Rectangle
  - Triangle


For this you will need data - which we will create in Python using PIL.

# Phases

There are 4 phases to this - and I think most Image classification projects.

## Data Generation/Acquisition

If you have been given a dataset from the internet, then you probably need to check the image sizes, quality etc. And decide on what size to process these images as.

We simple generate nice clean RGB data - and store them in a directory

```text
  \data
    \rect
    \circle
    \tri
```

This makes identifying the class of the object much easier.

When the data is generated. We can move to the next step.

## Prepare Data

I now want to load my data so that it is in a format that Keras can process.

To this end I do the following

  - Using tensorflow load the image
  - convert and rescale to 100,100 pixels 
  - image format is grayscale
 
Wish all the images loaded, and their classes converted from "circle","rect",tri" to 0,1,2 etc The data is saved as a Pickle file.

## Model the Data

It starts off rather predictably with **load the pickle file**, then the Keras model needs to be defined.

This is still rather an experimental cooking exercise, sometimes you add a layer and it works - other times it fails.

However when the **loss**, and **validation_loss** are looking reasonable. I generally stop messing with the model.

This is the graph I a referring to.

![](data:image/jpeg;base64,iVBORw0KGgoAAAANSUhEUgAAAX8AAAEJCAYAAAB8Pye7AAABX2lDQ1BJQ0MgUHJvZmlsZQAAKJFjYGBiSSwoyGFhYGDIzSspCnJ3UoiIjFJgf87AysDGwM5gzcCemFxc4BgQ4ANUwgCjUcG3awyMIPqyLsgslwfb/nWsZ1EyunQ6+cbSdSsx1aMArpTU4mQg/QeItZILikoYGBg1gOyA8pICELsCyBYpAjoKyO4BsdMh7AUgdhKEvQWsJiTIGcg+AWQLJGckpgDZN4BsnSQk8XQkdm5OaTLUDSDX86TmhQYDaT4glmEIZjBiMGdwYTBlsGQwAIYNdrUmYLXODPkMBQyVDEUMmQzpDBkMJQwKDI5AkQKGHIZUINuTIY8hmUGPQQfINgKaZsRgDApj9LBDiKW8ZGAw1mFgYFJGiGUKMDDsqgR6cxZCTOMuA4NQNAPDnoKCxKJEeIgyfmMpTjM2grAlmoBB7Pv//4taBgZeoE/+ff3//wf3//+/5zMwsAPt6F0LALR+YP9VNUzjAAAAVmVYSWZNTQAqAAAACAABh2kABAAAAAEAAAAaAAAAAAADkoYABwAAABIAAABEoAIABAAAAAEAAAF/oAMABAAAAAEAAAEJAAAAAEFTQ0lJAAAAU2NyZWVuc2hvdAuEnagAAAHWaVRYdFhNTDpjb20uYWRvYmUueG1wAAAAAAA8eDp4bXBtZXRhIHhtbG5zOng9ImFkb2JlOm5zOm1ldGEvIiB4OnhtcHRrPSJYTVAgQ29yZSA2LjAuMCI+CiAgIDxyZGY6UkRGIHhtbG5zOnJkZj0iaHR0cDovL3d3dy53My5vcmcvMTk5OS8wMi8yMi1yZGYtc3ludGF4LW5zIyI+CiAgICAgIDxyZGY6RGVzY3JpcHRpb24gcmRmOmFib3V0PSIiCiAgICAgICAgICAgIHhtbG5zOmV4aWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20vZXhpZi8xLjAvIj4KICAgICAgICAgPGV4aWY6UGl4ZWxZRGltZW5zaW9uPjI2NTwvZXhpZjpQaXhlbFlEaW1lbnNpb24+CiAgICAgICAgIDxleGlmOlBpeGVsWERpbWVuc2lvbj4zODM8L2V4aWY6UGl4ZWxYRGltZW5zaW9uPgogICAgICAgICA8ZXhpZjpVc2VyQ29tbWVudD5TY3JlZW5zaG90PC9leGlmOlVzZXJDb21tZW50PgogICAgICA8L3JkZjpEZXNjcmlwdGlvbj4KICAgPC9yZGY6UkRGPgo8L3g6eG1wbWV0YT4KUcu6HgAAQABJREFUeAHtXQd8VUXW/6dXSAKhdwhEivSmfosiCiqKHbG7urIq37p2111Fl9UVdV11xYaiixXbR1EBAelI71IjJJCEmhDSe953zrw3zxfIS3m5L7nvvnN+ebll5k75z9z/nDkzdybARgIRQUAQEAQEAb9CINCvciuZFQQEAUFAEFAICPlLRRAEBAFBwA8REPL3w0KXLAsCgoAgIOQvdUAQEAQEAT9EoNbkX1FRgfLy8ip/Ro8Zz5o1C/v27fO54igqKsKxY8fcpvvUqVPIzc312N3tg24cuFzclRmXp79IfXBnDNPT0xWO/oKX5NM/EKg1+Y8fPx7du3ev8ve3v/3NULT+9a9/YfPmzYaGWVNgSUlJePLJJ9GnTx9MnDixJu9nub/yyivo168fzj//fFxyySVISUlx+snKygLjN3jwYAwYMAAPPfQQSktLa+3u9FjHk48++qjK8uJyHDhwYB1Dc+99y5YteOyxx9x78NCFG6jFixfjxhtvRGJiIr755ps6hVRf3NeuXYshQ4bgd7/7nSrbb7/9tk7xi2dBwNQIkGZTK0lNTbXt379f/S699FLbPffc47w+fvx4rcKorSeOh17c2nqvtz/SDG3dunWz3XXXXTbO25133lmnMBctWmTr2rWr7YcffrDt3LnTduGFF9quuuoqZxhE9upecnKy7euvv7Z16dLF9s4779Ta3emxjieMoS6zf/zjH7aEhATn9YEDB+oYmnvv8+bNs/Xo0cO9Bw9dPv30Uxs1xrannnpKYfbVV1/VKaT64F5QUGCjxtz2+OOP20jztz344IOqjKlRr1MaxLMgYFYEaq35t2/f3qlFhoeHo2nTps7rli1bqgYuIyMDY8aMwdy5czFhwgT07t0bV155JehFwtatW3HDDTcoDap///645ZZbsGfPnkoN49NPP638P/zww2Cty1XuvfdevPzyy0qD5uefeOIJcHyusnz5chU/P18XiYqKwpIlS8CacseOHREQEFCXx8EaIWv9V1xxBcLCwpSZ4JdffgERLwoLCzF//nzcdttt6Ny5M06ePKnC1lpkTe51SsgZnmNjY51lFB8fj8DAQOc1NVbK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)

We now save the model - so we can use it when we need.


## Predict

Rather typically we load the trained model - and see with using the training/testing data files how accurate it is ... or if we have a new set of files - we could use that.

Please note: The **new** set of files will have to be in the same format as the trained data i.e. 100 by 100 grayscale. 

# Workbooks

You will find a workbook per phase in this repository.

Python Modules are (this may miss some - apologies)

  - keras
  - tensorflow
  - pillow (PIL)
  - numpy
  - glob
  - pickle
  - sklearn
  - matplotlib

  