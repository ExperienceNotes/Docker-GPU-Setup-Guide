# Docker 使用說明

###### tags: `Tool` `Docker` `Server`

## 建立 Docker Image

```bash=
docker build -f ./Ubuntu22.04-CUDA.Dockerfile -t ubuntu22.04-cuda:latest .
```
|     參數     | 說明                                                   |
|:------------:|:--------------------------------------------------------:|
| `-f`       | 指定 Dockerfile 路徑。                                 |
| `-t`       | 設定 Image 名稱與標籤。                                |

## 建立 Docker Container

```bash=
docker run --gpus all --shm-size=8G -d --name container_name -v your_data_path:/workspace -it your_container_name:your_tag
```
範例：
```bash=
docker run --gpus all --shm-size=8G -d --name Demo_container -v /data/Yolov9:/workspace -it ubuntu22.04-cuda:yolov9
```
| 參數         | 說明                                                   |
|:------------:|:--------------------------------------------------------:|
| `--gpus`   | 使用 GPU 設定，如 `all` 或 `--gpus 1`。               |
| `--shm-size` | 設定共享記憶體大小，預設為 64MB，例：`--shm-size=8G`。 |
| `-d`       | 背景運行容器。                                         |
| `--name`   | 指定容器名稱。                                         |
| `-v`       | 映射本地資料夾到容器。                                 |
| `-it`      | 進入容器的互動式 shell。                               |
| `-i`       | 持續打開容器的標準輸入通道，允許互動。                  |
| `-t`       | 分配虛擬終端進行互動。                                  |

## 進入容器 Shell

```bash=
docker exec -it container_name bash
```
| 參數         | 說明                                                      |
|:------------:|:--------------------------------------------------------:|
| `-i`       | 保持容器的標準輸入通道打開，允許與 Shell 進行互動。            |
| `-t`       | 分配一個模擬終端，提供互動環境。                              |

## 遠端監控服務器連接

### 創建遠端 Context

```bash=
docker context create my_context_name --docker "host=tcp://ip_address:<port>"
```
範例：
```bash=
docker context create Ph_server --docker "host=tcp://192.168.1.10:2376"
```
| 參數         | 說明                                                      |
|:------------:|:--------------------------------------------------------:|
| `--docker`       | 指定要連接的 Docker 配置。            |
| `host=tcp://ip_address:<port>`| 指定遠端 Docker 的 IP 地址和連接埠。             |

### 使用遠端 Context

```bash=
docker context use context_name
```
範例：
```bash=
docker context use Ph_server
```

## 參考
- https://medium.com/alberthg-docker-notes/docker%E7%AD%86%E8%A8%98-docker%E5%9F%BA%E7%A4%8E%E6%95%99%E5%AD%B8-7bbe3a351caf
- https://bobcares.com/blog/docker-shm-size/
- https://github.com/twtrubiks/docker-tutorial