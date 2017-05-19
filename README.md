## PID Controller Project
### David del Río Medina

---

## [Rubric](https://review.udacity.com/#!/rubrics/824/view) Points

#### 1. Describe the effect each of the P, I, D components had in your implementation.

* The *P* component of the controller steers the car in the direction of the given line the car should follow, with an angle proportional to the distance between the car and the line (cross track error).
The *P* component alone tends to make the car overshoot the angle, which causes big oscillations, as shown in [this video.](p-controller.mpeg)

* The *I* component corrects the systematic bias of the car. For instance, if the vehicle tends to steer slightly to the left when the input steering angle is zero. It takes into account the accumulated cross track error to apply a small correction to the steering.
In my implementation, all the good combinations of params found minimize this component, indicating that the bias of the car might be negligible. Giving more importance to the *I* component just makes the car oscillate more, specially at the beginning, when the accumulated cross track error is big due to the car starting away from the line.

* The *D* component smoothes the steering by taking into account the reduction of the cross track error between two periods of time, preventing the car to continuously overshoot the target and oscillate out of control.
The *D* component alone can make the car drive succesfully during a big section of the track, but its lack of responsiveness causes the vehicle to drive off the road in the steeper curves, as shown in [this video.](d-controller.mpeg)

#### 2. Describe how the final hyperparameters were chosen.

I found two parameters combinations that work correctly: [0.1, 0.0, 1.0] and [0.2, 0.001, 2.0].

The first one, [0.1, 0.0, 1.0], was found by manual tuning, using Sebastian Thrun's lesson as a guideline. With this set of parameters, the car can drive quite smoothly through the whole track, but it steers slow, causing the tires to step on the red and white edges in the tighter curves.

The second set of parameters, [0.2, 0.001, 2.0], was found by a quick and dirty implementation of grid search. With these parameters, the car oscillates much more than with the previous ones, but its better responsiveness makes it steer quicker and therefore drive closer to the center of the lane in the tighter curvers.

The implemented grid search algorithm tries to minimize the average cross track error plus the maximum cross track error. The idea is to find a combination of parameters that tries to minimize the cross track error during the whole trip, but also tries to prevent peaks in the cross track error that can lead to dangerous situations.
