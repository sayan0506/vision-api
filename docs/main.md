# Vision-API
The Vision API is a work-in-progress customized toolkit that enables detection of a variety of plant leaf diseases. The API uses models
that are deployed to a cloud server to interface with the end-users.

## Components
Vision API provides 11 python files, right from loading and exploring the data to saving the converted tflite model. The components are:
+ [data_loader](components/data_loader.md)
+ [data_preparation](components/data_preparation.md)
+ [data_analysis](components/data_analysis.md)
+ [environment_setup](components/environment_setup.md)
+ [Potato_model_build](components/Potato_model_build.md)
+ [model_training](components/model_training.md)
+ [train](components/train.md)
+ [evaluate](components/evaluate.md)
+ [save_model](components/save_model.md)
+ [tflite_conversion](components/tflite_conversion.md)
+ [main](components/main.md)

## Building Vision API
A guide to building the Vision API can be found [here](building.md).

## Upcoming Features
A list of planned features for the Vision API can be found [here](roadmap.md).

## Release History
A changelog of previous versions of Vision API can be found [here](changelog.md).