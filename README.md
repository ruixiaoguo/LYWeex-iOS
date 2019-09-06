# LYWeex-iOS
 WeexiOS工程项目模板
 
 ## 创建新模板
 > 简介：weexpack 是 weex 新一代的工程开发套件，是基于weex快速搭建应用原型的利器。它能够帮助开发者通过命令行创建weex工程，添加相应平台的weex app模版，并基于模版从本地、GitHub 或者 weex 应用市场安装插件，快速打包 weex 应用并安装到手机运行，对于具有分享精神的开发者而言还能够创建weex插件模版并发布插件到weex应用市场。
 
 
 ### 打包命令
 
 * weexpack create 项目名称(字母小写) -----创建Weex工程项目
 * weexpack platform add iOS  -----导入iOS模板
 * weexpack platform add android -----导入安卓模板
 * weexpack platform remove iOS/android -----删除iOS/安卓模板
 * weexpack platform list -----查看已安装的平台模版及版本
 * weexpack run ios/android-----打包应用并安装到设备运行 (依赖模拟器)
 * weexpack run web -------在 html5 平台运行
 * weexpack build ios ------构建ipa包
 * weexpack build web-----打包html5平台
 * /playground/build/ipa_build/ ----打完包成功之后，可以在此目录下获取ipa文件
 
 ### 插件使用者命令
 
 * weexpack plugin add/remove -----安装／移除 weex 插件，支持从本地、GitHub 或者 weex 应用市场安装插件。
 * weexpack plugin list -------查看已安装的插件及版本
 
 ### 插件开发者命令
 
 * weexpack plugin create -------生成weex插件模版，主要是配置文件和必需的目录。
 * weexpack plugin publish --------发布插件到weex插件市场。
 
  ## 集成week到现有iOS工程项目
  
  
  > 使用 CocoaPods 或 Carthage 可以方便地将 Weex 集成到自己的项目中。
  
  ### 1. 配置依赖,将 WeexSDK 添加到你的 Podfile 中。
  
      source 'git@github.com:CocoaPods/Specs.git'
      target 'YourTarget' do
      platform :ios, '8.0'
      pod 'WeexSDK', '0.20.1'
      end
      
      
### 2. 初始化 Weex,建议在 didFinishLaunchingWithOptions 回调中初始化 Weex

    // App configuration
    [WXAppConfiguration setAppGroup:@"Your app group"];
    [WXAppConfiguration setAppName:@"Your app name"];
    [WXAppConfiguration setAppVersion:@"Your app version"];

    //Initialize WeexSDK
    [WXSDKEngine initSDKEnvironment];

    //Register custom modules and components, optional.
    [WXSDKEngine registerComponent:@"myview" withClass:[MyViewComponent class]];
    [WXSDKEngine registerModule:@"mymodule" withClass:[MyWeexModule class]];

    //Register the implementation of protocol, optional.
    [WXSDKEngine registerHandler:[WXAppNavigationImpl new] withProtocol:@protocol(WXNavigationProtocol)];

    //Set the log level, optional
    [WXLog setLogLevel: WXLogLevelWarning];
    
    
### 3. 创建一个 Weex 实例

        #import <WeexSDK/WXSDKInstance.h>

        - (void)viewDidLoad
        {
        [super viewDidLoad];
        _instance = [[WXSDKInstance alloc] init];
        _instance.viewController = self;
        _instance.frame = self.view.frame;
        __weak typeof(self) weakSelf = self;
        _instance.onCreate = ^(UIView *view) {
        [weakSelf.weexView removeFromSuperview];
        weakSelf.weexView = view;
        [weakSelf.view addSubview:view];
        };
        _instance.onFailed = ^(NSError *error) {
        //process failure, you could open an h5 web page instead or just show the error.
        };
        _instance.renderFinish = ^ (UIView *view) {
        //process renderFinish
        };
        NSURL *url = [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"js"];
        [_instance renderWithURL:url options:@{@"bundleUrl":[self.url absoluteString]} data:nil];
        }
        
### 4. 销毁实例
        [instance destroyInstance];
        
        
### 具体请参考[https://weex.apache.org/zh/guide/develop/integrate-to-iOS-app.html#_1-配置依赖] (https://weex.apache.org/zh/guide/develop/integrate-to-iOS-app.html#_1-配置依赖)

  
  
