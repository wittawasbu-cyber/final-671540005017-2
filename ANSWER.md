\## Part A — ข้อมูลจาก image kamthornsa/ksu-promo:1.0



| คำถาม | คำตอบ | คำสั่งที่ใช้ |

| --- | --- | --- |

| Port | \_\_\_\_\_\_ | `docker inspect kamthornsa/ksu-promo:1.0` และ `docker history --no-trunc kamthornsa/ksu-promo:1.0` |

| CMD | \_\_\_\_\_\_ | `docker inspect --format "{{json .Config.Cmd}}" kamthornsa/ksu-promo:1.0` |

| Base image | \_\_\_\_\_\_ | `docker history --no-trunc kamthornsa/ksu-promo:1.0` |

