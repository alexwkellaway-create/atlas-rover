# Drivers

Hardware abstraction: motors, encoders, IMU, cameras, LiDAR. Each exposes a clean interface so the layers above never talk to a device directly.

This boundary is what lets the same navigation code run in simulation and on the real rover.
