```
services:

  minecraft-server:
    ports:
      - "25565:25565"

    environment:
      EULA: "TRUE"
      # # OLD OLD
      # # ConcMarkSweep is faster than default
      # JVM_XX_OPTS: "-server -XX:+UseConcMarkSweepGC -XX:+UseParNewGC -XX:+CMSIncrementalPacing -XX:ParallelGCThreads=2 -XX:+AggressiveOpts"
      # # OLD
      # # Reasonable tunings for G1GC
      # # Works in standard modern versions of Java
      # JVM_XX_OPTS: "-server -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:G1NewSizePercent=20 -XX:G1ReservePercent=20 -XX:MaxGCPauseMillis=50 -XX:G1HeapRegionSize=32M -XX:+AggressiveOpts -XX:ParallelGCThreads=4"
      # # OLD
      # # Really dumb tunings for G1GC
      # JVM_XX_OPTS: "-server -Xms4G -Xmx4G -XX:+UnlockExperimentalVMOptions -XX:+UseG1GC -XX:GCTimeRatio=99 -XX:MaxTenuringThreshold=4 -XX:G1NewSizePercent=60 -XX:InitiatingHeapOccupancyPercent=90 -XX:G1MixedGCLiveThresholdPercent=95 -XX:G1ReservePercent=7 -XX:G1MixedGCCountTarget=8 -XX:G1OldCSetRegionThresholdPercent=5 -XX:MaxGCPauseMillis=25 -XX:G1HeapRegionSize=32M -XX:G1ConcRefinementThreads=2 -XX:ParallelGCThreads=4 -XX:G1HeapWastePercent=70 -XX:+ParallelRefProcEnabled -XX:+AlwaysPreTouch -XX:+DisableExplicitGC -XX:+AggressiveOpts"

      # # Use The Z Garbage collector provided by the OpenJRE (Very fast!)
      # # Pause times do not exceed 10ms*
      # # pause times do not increase with the heap or live set size
      # # Handle heaps ranging from 8MB to 16TB in size
      # # ----------------------------------------------
      # # Is Concurrent, Region-based, Compacting, NUMA-aware, Using colored pointers, Using load barriers
      # # "At its core, ZGC is a concurrent garbage collector, meaning all heavy lifting work is done while Java threads
      # #     continue to execute. This greatly limits the impact garbage collection will have on your
      # #     application's response time
      # # ----------------------------------------------
      # # *(future goal of 1ms)
      # # ----------------------------------------------
      # # In layman's terms, this garbage collector greatly helps reduce Minecraft server lag. Very nice to have
      JVM_XX_OPTS: "-server -Xms4G -Xmx4G -XX:+UnlockExperimentalVMOptions -XX:+UseZGC -Xlog:gc"
      MEMORY: "4G"

    # use openj9 to get OpenJRE which we need for the Z Garbage Collector
    image: itzg/minecraft-server:openj9
    # image: itzg;minecraft

    container_name: mc
    
    tty: true
    stdin_open: true
    restart: always
    
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /minecraft:/data

```
