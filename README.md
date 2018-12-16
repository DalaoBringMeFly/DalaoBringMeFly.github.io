# 「大佬带我飞」项目文档

## 前情提要

这是我们小组简单 web 服务与客户端开发实战。

- 我们服务端选择使用 **GraphQL** 来构建服务。
- 我们客户端选择使用 Vue 富客户端 js 框架来构建我们的前端。

我们的本次目标为：***复制 https://swapi.co/ 网站***

我们实现了：

1. 获取该网站所有资源与数据
2. 资源的授权访问
3. 使用 boltDB 
4. 界面不能少于 3 个界面
5. 服务 API 不能少于 6 个
6. 基于 **GraphQL** 与 **Vue**

## 快速上手

### 后端安装

#### Step 1. 下载

```shell
$ go get -u github.com/DalaoBringMeFly/BringMeFlyServer
```

或者进入[项目地址](https://github.com/DalaoBringMeFly/BringMeFlyServer)，将文件放入自己的 Go 工作空间。

#### Step 2. 运行

```shell
# 进入 BringMeFlyServer 文件

$ go run main.go
```

获得提示

```shell
Now server is running on port 5656
```

#### Step 3. 测试

```shell
$ curl -g 'http://localhost:5656/graphql?query={film(id:1){title}}'
{"data":{"film":{"title":"Return of the Jedi"}}}
```

成功得到返回的数据～

此时，服务器已经成功运行，我们不要关闭终端，我们将要开始前端的安装配置。

### 前端安装

#### Step 1. 下载

```shell
git clone https://github.com/DalaoBringMeFly/BringMeFlyWeb
```

进入[项目地址](https://github.com/DalaoBringMeFly/BringMeFlyWeb)，下载文件，把文件放的合适的位置。

#### Step 2. 安装依赖

```shell
# 进入 BringMeFlyWeb 文件
$ cd BringMeFlyWeb
$ npm install
```

通过 npm 下载安装项目所需要的 `node_modules` ~~（黑洞）~~ 。

#### Step 3. 运行

```shell
# 使用开发模式运行
# 这时候会热加载，所有修改可以立刻得到
# localhost:8080

$ npm run dev
```

#### Step 4. 测试

进入浏览器，打开 `localhost:8080`

![屏幕快照 2018-12-16 下午7.18.14](media/15449501628455/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-12-16%20%E4%B8%8B%E5%8D%887.18.14.png)

完成！

此时，前后端都已经跑了起来。

## 设计分享

### 后端 API 设计

_基于 **Go**、**boltdb**、**graphql**_

**film** 👇

```go
type Film struct {
	Producer      string `json:"producer"`
	Title         string `json:"title"`
	EpisodeID     int    `json:"episode_id"`
	OpeningCrawl  string `json:"opening_crawl"`
	Director      string `json:"director"`
	CharacterURLs []int  `json:"characters"`
	PlanetURLs    []int  `json:"planets"`
	StarshipURLs  []int  `json:"starships"`
	VehicleURLs   []int  `json:"vehicles"`
	SpeciesURLs   []int  `json:"species"`
	Created       string `json:"created"`
	Edited        string `json:"edited"`
	ReleaseData   string `json:"release_date"`
}
```

**person** 👇

```go
type Person struct {
	Name      string `json:"name"`
	Height    string `json:"height"`
	Mass      string `json:"mass"`
	HairColor string `json:"hair_color"`
	SkinColor string `json:"skin_color"`
	EyeColor  string `json:"eye_color"`
	BirthYear string `json:"birth_year"`
	Gender    string `json:"gender"`
	Homeworld int    `json:"homeworld"`
	Created   string `json:"created"`
	Edited    string `json:"edited"`
}
```

**planet** 👇

```go
type Planet struct {
	Name           string `json:"name"`
	RotationPeriod string `json:"rotation_period"`
	OrbitalPeriod  string `json:"orbital_period"`
	Diameter       string `json:"diameter"`
	Climate        string `json:"climate"`
	Gravity        string `json:"gravity"`
	Terrain        string `json:"terrain"`
	SurfaceWater   string `json:"surface_water"`
	Population     string `json:"population"`
	Created        string `json:"created"`
	Edited         string `json:"edited"`
}
```

**species** 👇

```go
type Species struct {
	Name            string `json:"name"`
	Classification  string `json:"classification"`
	Designation     string `json:"designation"`
	AverageHeight   string `json:"average_height"`
	SkinColors      string `json:"skin_colors"`
	HairColors      string `json:"hair_colors"`
	EyeColors       string `json:"eye_colors"`
	AverageLifespan string `json:"average_lifespan"`
	Homeworld       int    `json:"homeworld"`
	Language        string `json:"language"`
	PeopleURLs      []int  `json:"people"`
	Created         string `json:"created"`
	Edited          string `json:"edited"`
}
```

**starship** 👇

```go
type Starship struct {
	HyperdriveRating string `json:"hyperdrive_rating"`
	MGLT             string `json:"MGLT"`
	StarshipClass    string `json:"starship_class"`
	PilotURLs        []int  `json:"pilots"`
}
```

**vehicle** 👇

```go
type Vehicle struct {
	VehicleClass string `json:"vehicle_class"`
	PilotURLs    []int  `json:"pilots"`
}
```

你可以遵循着 GraphQL 的风格来请求所需要的信息。

例如：

```shell
$ curl -g 'http://localhost:5656/graphql?query={film(id:1){title}}'
{"data":{"film":{"title":"Return of the Jedi"}}}
```

### 前端 Web 设计

_基于 **Vue**、**Babel**、**eslint**_

**查询页面**

![屏幕快照 2018-12-16 下午7.45.35](media/15449501628455/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-12-16%20%E4%B8%8B%E5%8D%887.45.35.png)

**关于页面**

![屏幕快照 2018-12-16 下午7.46.55](media/15449501628455/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-12-16%20%E4%B8%8B%E5%8D%887.46.55.png)

**介绍页面**

![屏幕快照 2018-12-16 下午7.48.11](media/15449501628455/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202018-12-16%20%E4%B8%8B%E5%8D%887.48.11.png)



