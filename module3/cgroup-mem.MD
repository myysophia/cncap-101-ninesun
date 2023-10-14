### 在 cgroup memory 子系统目录中创建目录结构

```sh
cd /sys/fs/cgroup/memory
mkdir memorydemo
cd memorydemo
```

### 运行 malloc（在 linux 机器 make build）
[root@hadoop100 /opt/go]#./memoryconsumption
Allocating 100Mb memory
Allocating 200Mb memory
Allocating 300Mb memory
Killed

### 查看内存使用情况

```sh
watch 'ps -aux|grep memoryco|grep -v grep'
```

### 通过 cgroup 限制 memory

### 把进程添加到 cgroup 进程配置组

```sh
echo `ps -ef|grep memoryco |grep -v grep|awk '{print $2}'` > cgroup.procs
```

### 设置 memory.limit_in_bytes

```sh
echo 104960000 > memory.limit_in_bytes # 限制为100MB
```

### 等待进程被 oom kill
最终在系统的mssage中可以看到
#[root@hadoop100 /sys/fs/cgroup/memory/memorydemo]#tail -f /var/log/messages | grep oom
#Apr 21 15:25:20 hadoop100 kernel: memoryconsumpti invoked oom-killer: gfp_mask=0xd0, order=0, oom_score_adj=0
#Apr 21 15:25:20 hadoop100 kernel: [<ffffffffa73c254d>] oom_kill_process+0x2cd/0x490
#Apr 21 15:25:20 hadoop100 kernel: [<ffffffffa74416cc>] mem_cgroup_oom_synchronize+0x55c/0x590
#Apr 21 15:25:20 hadoop100 kernel: [ pid ]   uid  tgid total_vm      rss nr_ptes swapents oom_score_adj name
