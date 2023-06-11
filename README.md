# intermediate-experiment-language

This is the specification for intermediate experiment language (.ixl) files, which are used by [GLUE](https://github.com/VirtualBrainLab/Glue) to build experiments.

# Experiment Graph

An experiment is a directed graph that starts from a `ExperimentStart` node and leads to an `ExperimentEnd` node. 

# Nodes

## Experiment Nodes

These nodes exist in all experiments and define the trial-by-trial logic of the experiment itself.

- Loading: Triggered when the program shows it's loading screen. Useful for delayed asset loading or complicated calculations.
- **ExperimentStart** (required): Experiment start is triggered when the program is finished loading and should start the main logic.
- **ExperimentEnd** (required): Calling experiment end should quit the program.
- BlockStart_X:
- BlockEnd_X:
- TrialStart_X:
- TrialEnd_X:
- Update: Called on each frame
- FixedUpdate: For physics updates

## Utility Nodes

- Time: `deltaTime` since the last frame, deltaTime since the trial start, block start, etc, `delay(ms)`.
- Math: Basic mathematical functions: `add`, `subtract`, `multiply`, `divide`, `random` numbers

## Object Nodes

- Transform: A base transform node has no 2D/3D model, but defines a position, rotation, and scale. Transforms can have children/parents.
- Object2D (Sprite): 2D object nodes has a transform as well as a set of properties that control their texture, shader, etc
- Object3D (Mesh): 3D object nodes also have a mesh (and eventually, animations)
- UI: User interface elements, such as text
- Camera: Defines the position of an orthographic or perspective camera

## Data Nodes

Data nodes capture custom information. By default, the program logs the interval between the experiment, block, and trial start and ends nodes (i.e. reaction times). Each set of data is stored in its own Experiment/Block/Trial dataframe, which is saved a CSV file. 

- ExperimentData/BlockData/TrialData: Takes as input a serializable value and a name and saves these as a new column in the corresponding datafarme.

# Data

By default, the program should log 

# API

The application should be controllable from triggers sent via an external application, through ZeroMQ. Data is sent as a flat byte[], where the first 4 bytes identify the message type and any data that need to be sent. 

## Triggers

### Experiment triggers (message type only)
- start [0000]: Triggers the ExperimentStart node and begins the experiment
- stop [0001]: Ends the experiment
- pause [0002]: Pauses the experiment

### Data triggers

- get data [001x]: Requests all of the current data for x=0/1/2=experiment/block/trial
