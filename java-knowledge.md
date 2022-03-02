## OOM trouble shooting
symptons
* failure or slowness of java application
* OMM error of java heap space
* CPU utilizaiton get higher
causes
* heap capacity setting , Xms Xmx
* java program memory leak
* excessive applicaiton processing workload
diagnostic
* jvm verbose: gc logs
* Oracle mission control
* APM technologies
common solutons:
* increase xmx
* monitor profile and resolve application memory leaks
* optimize and reduce java applicaiton memory footprint
* split the workload (vertically or horizontally)
