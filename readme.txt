˵����
��������޸���ܶ��ǹ�˵��������˵��������Ȼ��ˣ��ҾͶ�����һ����
��ǰʹ�ð����andfix���ɵ����ǻ����׹�����Ŀ�����ƴ���������߷��֣�������80%-90%�������ʴﵽ0.1���ϣ�����ʹ�á�
��Ѷ΢�ŵ�tiknerҲ��������������ӣ��ĵ����ӣ��Ҳ�֧�ּӹ̣�����360���ְ�׿�г���������360�ӹ�����ӹ̺���ܷ��������ݲ���tinker��
���ŵ�Robust��˵�ȽϿ��ף����ǲ���Դ�������ò��ˡ�
���ڵ���ĳ����ʦ������һ��nuwa���õ���DexClassLoader������ϵͳ�Դ���api�������Ժã��ȶ��Ըߡ�
�������ߵ�nuwa����֧��gradle 1.2.3������Ŀ��Ŀ¼build.gradle�е�classpath 'com.android.tools.build:gradle:1.2.3'����
���ڶ�2.3��ʱ���ˣ�instant-run�ȶ���Ҫ�°�gradle����֧�֣����Ժܶ��˶���Ը���á�����ûά������Ϊ���Լ���ҵȥ�ˣ���������û����Щ�����˰ɡ�
����nuwaԭ��ɿ��������򵥣���Ҫ�˷�ǰ�˵�Ѫ��������Ҫ������û����������ð�������Ҳû��Ū�ס����á��Ĳ���������Ұ�����gradle���������һ����֧��1.x�����ڵ�2.x�汾������������֮ǰ�Ĳ���ϸ�ڡ�

����˵����������(Teamlib SDK�ڲ�����library��ĿĬ���Ѽ��ɣ�ֻ��ִ�д򲹶�������������ܵ������ɷ�ʽ)��
1.����pluginĿ¼��app/plugin

-------------------------------------------------------------------------------------------------------------------------------

2.��Ҫ��������Ȩ��
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>

-------------------------------------------------------------------------------------------------------------------------------
	
3.׷��proguard-rules.pro�еĻ������ò��ҿ�����������������Ŀ�������õ�ĳһ�У�����һ�в����ظ����ƹ�ȥ��
buildTypes {
	release {
		minifyEnabled true
		proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
	}
}

-------------------------------------------------------------------------------------------------------------------------------

4.app/build.gradle��Ӳ��������
buildscript {
    repositories {maven {url uri('./plugin')}}
    dependencies {classpath 'cn.jiajixin.nuwa:buildsrc:2.0'}
}
apply plugin: 'cn.jiajixin.nuwa'


compile 'cn.jiajixin.nuwa:nuwa:1.0.0'

-------------------------------------------------------------------------------------------------------------------------------

5.�Զ���Application��д�����������
@Override
protected void attachBaseContext(Context base) {
	super.attachBaseContext(base);
	Nuwa.init(this);
	// /sdcard/patch_1.jar��������VERSION_CODE���ְ汾�����°�APPҲ������Щ�ϲ���������Ӧ�ÿ����ú�̨��һ���������ز����Ľӿڣ������������Application�������Ǽ�����Activity������ҾͲ���˵��
	Nuwa.loadPatch(this, Environment.getExternalStorageDirectory().getAbsolutePath() + File.separator + "patch_" + BuildConfig.VERSION_CODE + ".jar");
	//������Ĵ��뷽������������棬����android.support.multidex.MultiDex.install(this);
}

-------------------------------------------------------------------------------------------------------------------------------
	
6.rebuild��ͬ���󣬴��release�汾apk������app/build/outputs������nuwa�ļ��У����ݺã�����ŵ�d:/nuwa��ÿ��release��ʽ����ʱһ��Ҫ���ݵ�ǰ�����apk��Ӧ��Դ�룬�Ͷ�Ӧ��nuwa�ļ��С���װ�������õ�apk��

-------------------------------------------------------------------------------------------------------------------------------

7.�Ȳ��Ե����������������һ�γ���bug��apk��Ӧ��Դ������ϣ��޸�����һ����������������һ��java�࣬����ɾ��ĳ����������֧���������������ࡢ���������������������޸��ࡢ�����޸���Դ�ļ��������޸�Application���ࣩ
��android studio�ײ�Terminal���������룺gradlew nuwaReleasePatch -P NuwaDir=D:/nuwa���״α������Ҫ����һЩ����������ͷǳ��죬����D:/nuwa���ǵ�6�����б�����ļ���
����ִ����ɻ���֡�BUILD SUCCESSFUL����Ȼ��ȴ����룬���������ɵ�app/build/outputs/nuwa�ļ����е�patch.jar��
�����������ŵ���5��ָ����λ�ã�/sdcard/patch_��İ汾��.jar��ɱ��APP�����������ɿ�����Ķ���Ĺ��ܣ���finish���У���ҪkillProgress����

-------------------------------------------------------------------------------------------------------------------------------

8.��һ��û�оͺ��Բ��ÿ������ڶ�����������⡣��������������������
	productFlavors {
        qihoo360//360����
                {
                    manifestPlaceholders = [channelID_td_analytics: "360�г�"]
                    buildConfigField "String", "channelId_talkingdata", "\"xxxxxxx\""
                    buildConfigField "String", "channel", "\"qihoo360\""
                }
        tecent//Ӧ�ñ�
                {
                    manifestPlaceholders = [channelID_td_analytics: "Ӧ�ñ�"]
                    buildConfigField "String", "channelId_talkingdata", "\"xxxxx\""
                    buildConfigField "String", "channel", "\"tecent\""
                }
    }
���ɲ���������죬�м����Qihoo360���ɣ�
gradlew nuwaQihoo360ReleasePatch -P NuwaDir=D:/nuwa
