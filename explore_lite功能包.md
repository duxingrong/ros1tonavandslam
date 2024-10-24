# explore_lite
网址为[explore_lite](http://wiki.ros.org/explore_lite)

他主要是可以订阅的地图要么是slam建立的地图，或者是move_base导航的地图，如果是slam的地图的话，就需要给参数`min_frontier_size`一个合理的数字，如果是costmap的话需要设置track_unknow_space: true，究竟是接收哪个地图好得以实践来检验

与move_base是通过Action起的服务，会订阅/map，/map_update 或者 /move_base/global_costmap/costmap ，/move_base/global_costmap/costmap_update ，发布的话题有frontiers (visualization_msgs/MarkerArray) 

使用的话主要看robot_base_frame 是base_link 还是 base_footprint


~visualize (bool, default: false)

    Specifies whether or not publish visualized frontiers. 

~planner_frequency (double, default: 1.0)

    Rate in Hz at which new frontiers will computed and goal reconsidered. 

~progress_timeout (double, default: 30.0)

    Time in seconds. When robot do not make any progress for progress_timeout, current goal will be abandoned. 

~potential_scale (double, default: 1e-3)

    Used for weighting frontiers. This multiplicative parameter affects frontier potential component of the frontier weight (distance to frontier). 

~orientation_scale (double, default: 0)

    Used for weighting frontiers. This multiplicative parameter affects frontier orientation component of the frontier weight. This parameter does currently nothing and is provided solely for forward compatibility. 

~gain_scale (double, default: 1.0)

    Used for weighting frontiers. This multiplicative parameter affects frontier gain component of the frontier weight (frontier size). 

~transform_tolerance (double, default: 0.3)

    Transform tolerance to use when transforming robot pose. 

~min_frontier_size (double, default: 0.5)

    Minimum size of the frontier to consider the frontier as the exploration goal. In meters.
