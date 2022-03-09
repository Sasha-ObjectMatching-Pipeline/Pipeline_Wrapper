FROM ros:melodic-perception-bionic
# uJCpNeVz1Q3SaowTUyxF
SHELL ["/bin/bash", "-c"]
ENV HOSTNAME=localhost
ARG TIDYUPVISION_REPO="-b matching_refactoring https://github.com/Sasha-ObjectMatching-Pipeline/tidyupvisionpipeline.git"
ARG MATCHING_REPO="https://github.com/Sasha-ObjectMatching-Pipeline/obj_det_ppf_matching.git"
ARG MSGS_REPO="https://github.com/Sasha-ObjectMatching-Pipeline/obj_det_ppf_matching_msgs.git"


RUN apt-get update && apt-get -qq -y --allow-unauthenticated install git vim python-catkin-tools python-pip curl

RUN echo "deb http://packages.ros.org/ros/ubuntu bionic main" > /etc/apt/sources.list.d/ros-latest.list
RUN curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -

RUN apt-get update && apt-get -qq -y --allow-unauthenticated install \ 
	ros-melodic-mongodb-store ros-melodic-ros-numpy python-backports.functools-lru-cache tmux \
	ros-melodic-move-base-msgs ros-melodic-actionlib-msgs ros-melodic-actionlib \
	libcmph-dev
WORKDIR /home/v4r/catkin_ws/src

RUN git clone ${MATCHING_REPO}
RUN git clone ${MSGS_REPO}
RUN git clone ${TIDYUPVISION_REPO}

RUN git clone --branch v0.4.0 https://github.com/google/glog.git
WORKDIR /home/v4r/catkin_ws/src/glog  
RUN cmake -H. -Bbuild -G "Unix Makefiles" && cmake --build build && \
   	cmake --build build --target install 
WORKDIR /home/v4r/catkin_ws/src   	
RUN rm -rf glog

#RUN pip install --upgrade setuptools
#RUN pip install pyrsistent==0.16.1 transforms3d==0.3.1 open3d_ros_helper==0.2.0.3 jsonschema==2.4 
#RUN pip install open3d==0.9.0.0
#RUN pip install scikit-image==0.14.5
RUN mv tidyupvisionpipeline/v4r_ppf obj_det_ppf_matching/
WORKDIR /home/v4r/catkin_ws/
RUN . /opt/ros/melodic/setup.sh && catkin build
#RUN sysctl -w net.ipv6.conf.all.forwarding=1

