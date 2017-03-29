PROJECT ( Hello )
INCLUDE_DIRECTORIES( ../../library/include )
ADD_EXECUTABLE( hello main.cpp )
TARGET_LINK_LIBRARIES( hello foo )
