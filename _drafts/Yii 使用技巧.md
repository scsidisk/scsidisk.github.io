Yii 使用技巧
===========


### $form->dropDownlist

    <?php
    echo $form->dropDownlist($model,'stat',array(
                '1' => '申请中',
                '2' => '审核通过',
                '3' => '暂停资格',
                '4' => '永久封停',
                '5' => '拒绝申请',
              ));
    ?>

### CActiveDataProvider写法

    // 写法1
    $criteria = array(
            'condition'=>'status=1',
            'order'=>'create_time DESC',
            'with'=>array('author'),
    );
    // 写法2
    $criteria = new CDbCriteria;
    //$criteria->select='title';  // 只选择 'title' 列
    //$criteria->condition='postID=:postID';
    //$criteria->params=array(':postID'=>10);
    $criteria->order = 'id desc';
    //$criteria->with = 'ext';
    //var_dump($criteria);exit;

    // CActiveDataProvider
    $dataProvider=new CActiveDataProvider('User', array(
        'criteria'=>$criteria,
        'pagination' => array('pageSize' => 20),
    ));

### heightcharts柱状图

    $this->Widget('ext.highcharts.HighchartsWidget', array(
       'options'=>array(
            'title' => array('text' => '月度报表'),
            'xAxis' => array(
                'categories' => array('销售1', '销售2', '销售3'),
            ),
            'yAxis' => array( 'title' => array('text' => null) ),
            'tooltip' => array(
                'shared' => true,
                'crosshairs' =>true
            ),
            'series' => array(
                array(
                    'type' => 'column',
                    'name' => '销售量',
                    'data' => array(12330, 9872, 11288),
                ),
            ),
            'exporting' => array('enabled' => false),
            'theme' => 'jasen',
            'chart' => array(
                'height' => '300',
            ),
       )

    ));

### 日志记录

Yii的自带组件有一个很实用的日志记录组件，使用方法可以参考Yii官方文档：http://www.yiiframework.com/doc/guide/1.1/zh_cn/topics.logging，在文档中提到，只要我们在应用程序配置文件中配置了log组件，那么就可以使用

    Yii::log($msg, $level, $category);

进行日志记录了。
配置项示例如下：

    array(
        ......
        'preload'=>array('log'),
        'components'=>array(
            ......
            'log'=>array(
                'class'=>'CLogRouter',
                'routes'=>array(
                    array(
                        'class'=>'CFileLogRoute',
                        'levels'=>'trace, info',
                        'categories'=>'system.*',
                    ),
                    array(
                        'class'=>'CEmailLogRoute',
                        'levels'=>'error, warning',
                        'emails'=>'admin@example.com',
                    ),
                ),
            ),
        ),
    )

log组件的核心是CLogRouter，如果想使用多种方式记录日志，就必须配置routes，可用的route有：

- CDbLogRoute: 将信息保存到数据库的表中。
- CEmailLogRoute: 发送信息到指定的 Email 地址。
- CFileLogRoute: 保存信息到应用程序 runtime 目录中的一个文件中。
- CWebLogRoute: 将 信息 显示在当前页面的底部。
- CProfileLogRoute: 在页面的底部显示概述（profiling）信息。

### Yii CDbCriteria 常用方法


注：$c = new CDbCriteria();

是ActiveRecord的一种写法，使ActiveRecord更加灵活，而不是手册中DAO（PDO）和Query Builder。

这是Yii CDbCriteria的一些笔记和常用用法：

    $criteria = new CDbCriteria;
    $criteria->addCondition("id=1"); //查询条件，即where id = 1
    $criteria->addInCondition('id', array(1,2,3,4,5)); //代表where id IN (1,23,,4,5,);
    $criteria->addNotInCondition('id', array(1,2,3,4,5));//与上面正好相法，是NOT IN
    $criteria->addCondition('id=1','OR');//这是OR条件，多个条件的时候，该条件是OR而非AND
    $criteria->addSearchCondition('name', '分类');//搜索条件，其实代表了。。where name like '%分类%'
    $criteria->addBetweenCondition('id', 1, 4);//between 1 and 4

    $criteria->compare('id', 1);
    //这个方法比较特殊，他会根据你的参数自动处理成addCondition或者addInCondition，
    //即如果第二个参数是数组就会调用addInCondition

    $criteria->addCondition("id = :id");
    $criteria->params[':id']=1;

    $criteria->select = 'id,parentid,name'; //代表了要查询的字段，默认select='*';
    $criteria->join = 'xxx'; //连接表
    $criteria->with = 'xxx'; //调用relations
    $criteria->limit = 10;    //取1条数据，如果小于0，则不作处理
    $criteria->offset = 1;   //两条合并起来，则表示 limit 10 offset 1,或者代表了。limit 1,10
    $criteria->order = 'xxx DESC,XXX ASC' ;//排序条件
    $criteria->group = 'group 条件';
    $criteria->having = 'having 条件 ';
    $criteria->distinct = FALSE; //是否唯一查询

用法一：完全sql语句

    $c = new CDbCriteria();
    $c->select = 't.id, t.created_at, t.outsource_id, t.user_id, t.operate, t.content';
    $c->join = 'LEFT JOIN outsource ON outsource.id=t.outsource_id';
    $c->condition = 'outsource.idc_id IN(' . implode(',', $idc_ids)  . ')';

    if($last_log_id) {
        $c->condition .= " AND t.id > $last_log_id";
    }

    $c->limit = 20;
    $c->order = 't.id DESC';

    $logs = OutsourceProcessLog::model()->findAll($c);

用法二：部分sql语句

### ccgridview自定义按钮

    array(
        'class'=>'CButtonColumn',
        'template' => '{apply}',
        'buttons' => array(
            'apply' => array(
                'label' => '通过',
                'imageUrl' => '/images/icon_apply.gif',
                'url' => 'Yii::app()->controller->createUrl("post/apply",array("id"=>$data->primaryKey))',
                'click' => 'function(){return confirm("确定要通过吗？");}',
            ),
        ),
    )

### CGridView ajax提交,并刷新grid-view

按钮部分

    array(
        'class'=>'CButtonColumn',
        'template' => '{confirm}',
        'buttons' => array(
            'confirm' => array(
                'label' => '通过',
                'imageUrl' => '/images/icons/icon_approve.png',
                'url' => 'Yii::app()->controller->createUrl("post/confirm",array("id"=>$data->primaryKey))',
                //'click' => 'function(){return confirm("确定要通过这个文章吗？");}',
                'options' => array('class'=>'confirm'),
            ),
        ),
    )

js代码

页面中如果没有下面的js需要添加 jquery.yiigridview.js


按钮的js

    <script type="text/javascript">
    /*<![CDATA[*/
    jQuery(function($) {
    jQuery('#user-grid a.confirm').live('click',function() {
         if(!confirm('确定要通过这个文章吗?')) return false;
         var th=this;
         var afterConfirm=function(){};
         $.fn.yiiGridView.update('user-grid', {
              type:'POST',
              url:$(this).attr('href'),
              success:function(data) {
                   $.fn.yiiGridView.update('user-grid');
                   afterConfirm(th,true,data);
              },
              error:function() {
                   afterConfirm(th,false);
              }
         });
         return false;
    });
    });
    /*]] > */
    </script>

### 使用SQL语句来填充CGridView

使用SQL语句来填充CGridView

    $count=Yii::app()->db->createCommand('SELECT COUNT(*) FROM tbl_user')->queryScalar();
    $sql='SELECT * FROM tbl_user';
    $dataProvider=new CSqlDataProvider($sql, array(
        'id'=>'user',
        'totalItemCount'=>$count,
        'sort'=>array(
            'attributes'=>array(
                 'id', 'username', 'email',
            ),
        ),
        'pagination'=>array(
            'pageSize'=>10,
        ),
    ));
    // $dataProvider->getData() will return a list of arrays.

在view视图中使用和CActiveDataProvider使用的方法一样

### 更改gridview pageSize

可以用以下三种方法

1.在view里面

    $provider=$model->search();
    $provider->pagination->pageSize=20;
    $this->widget('zii.widgets.grid.CGridView', array(
        'id'=>'link-grid',
        'dataProvider'=>$provider,
    ));

2.在model的search方法里

    return new CActiveDataProvider(get_class($this), array(
        'criteria'=>$criteria,
            'pagination'=>array(
                'pageSize'=> 5,
            ),
    ));

3.在controller里

    $dataProvider=new CActiveDataProvider('Post', array(
            'pagination'=>array(
            'pageSize'=>Yii::app()->params['postsPerPage'],
        ),
        'criteria'=>$criteria,
    ));

### Yii radio button list example

Here is an activeRadioButtonList example for rendering a set of radio buttons for gender selection

    //in your view
    echo $form->radioButtonList($person,'gender_code',array('m'=>'Male','f'=>'Female'));

If you don’t want the <br/> tag in between radio items, you can use the separator option, example

    //in the view
    echo $form->radioButtonList($person,'gender_code',
      array('m'=>'Male','f'=>'Female'),array('separator'=>''));

This way the seperator will render as an empty string.

    echo $form->radioButtonList(
        $model,
        'is_pass',
        array('0'=>'否','1'=>'是'),
        array('separator'=>'','labelOptions'=>array(
            'style'=>"display:inline-block;width:auto;float:none;margin-right:10px"
        ))
    )

官方该方法参数是这样解释的：

    radioButtonList(CModel $model, string $attribute, array $data, array $htmlOptions=array())

前三个参数都没得说，关键就是在第四个参数htmlOptions中的几个预留键['template'], ['separator'], ['encode']和['labelOptions']。

除了上面4个保留键，其他的键值对应直接算成radioButton的HTML属性。

- ['separator']默认为’<br />’，也就是每个radioutton默认为一行，想要所有radioButton在一行的直接让此属性为空就行了。
- ['template']默认为’{input} {label}’，也就是按钮在前，label在后。自定义改变之。
- ['encode']，是否编码HTML标签。默认为’true’
- ['labelOptions']，此选项为生成的label的样式。

### 使用CSqlDataProvider来改善你前台分页的性能

在网站开发时，我们使用CActiveDataProvider简单几行代码就完成拉分页功能，通常我是只用于管理后台的开发，如果前台访问量大时使用 CActiveDataProvider，将会存在很大性能问题，这篇文章我们将介绍使用CActiveDataProvider结合DAO模式来改善前 台分页列表的性能。

首先我们创建一个抽像DAO父类，所有子类都继承此类

    abstract class AbstractDAO extends CComponent
    {

        protected $_db;
        protected $_keyFileds;
        protected $_pagerSql;

        public function getDb()
        {
            if (!isset($this->_db)) {
                $this->_db = Yii::app()->db;
            }
            return $this->_db;
        }

        /**
         * 获得使用CSqlDataProvider keyField
         */
        public function getKeyFileds()
        {
            return $this->_keyFileds;
        }

        /**
         * 获得使用CSqlDataProvider keyField
         */
        public function getPagerSql()
        {
            return $this->_pagerSql;
        }

    }

接着我们再写一个具体的实现DAO，下面以CommentDAO为例:

    class CommentDAO extends AbstractDAO
    {

        private $_table = "comment";

        //分页需要显示的字段
        protected $_keyFileds = "id,created,consume,star,content";
        //分页SQL
        protected $_pagerSql = "
            SELECT id, c.created, c.consume, star, content
            FROM comment c
                LEFT OUTER JOIN post s ON (c.post_id=s.id)
            WHERE (c.status = 1 AND c.post_id=:post_id)
            ORDER BY c.created DESC
        ";

        public function counterComments($post_id)
        {
            $sqlCount = "SELECT COUNT(id) FROM " . $this->_table . "  WHERE status = 1 ";

            $querCount = $this->_db->createCommand($sqlCount);
            $querCount->bindParam(":store_id", $post_id, PDO::PARAM_INT);
            return $querCount->queryScalar();
        }

        //继续写你访问数据库的函数代码

    }

现在我们在controller action中就可以这样使用:

    $commentDAO=newCommentDAO();
    //统计总数
    $count=$commentDAO-> counterComments(0);
    //分页SQL
    $sql=$commentDAO-> getPagerSql(0);
    //显示的字段
    $keyFiles=$storeCommentDAO-> getKeyFileds();

    $dataProvider=newCSqlDataProvider($sql,array(
        'totalItemCount'=>$count,
        'keyField'=>$keyFiles,
        'pagination'=>array(
        'pageSize'=>15,
        ),
        'sort'=>false,//注意此处，如果不禁用排序，头部标题将显示为灰色
    ));

剩下的视图页面就同原来一样使用.

    <?php$this->widget('zii.widgets.CListView',array(
        'dataProvider'=>$dataProvider,
        'itemView'=>'_listview',
        'template'=>"{summary}\n{pager}\n{items}\n{pager}",
        'ajaxUpdate'=>'comments_container',
        'ajaxVar'=>'ajax',
        'viewData'=>array("commentUserId"=>Yii::app() -> user -> id,'isGuest'=>Yii::app() -> user ->isGuest),
        'pager'=>array(
            'class'=>'CLinkPager',
            'header'=>'',
            'firstPageLabel'=>'<<',
            'prevPageLabel'=>'<',
            'nextPageLabel'=>'>',
            'lastPageLabel'=>'>>',
            'maxButtonCount'=>5,
        )

    )); ?>

现在可以去测试一下你的网站性能是不是能改进.

### CActiveDataProvider, CArrayDataProvider, CSqlDataProvider做为gridview挂件的数据提供者的使用经验

首先构造测试数据 创建两个表

    --
    -- 表的结构 `tbl_user`
    --
    CREATE TABLE IF NOT EXISTS `tbl_user` (
      `uid` int(11) NOT NULL auto_increment,
      `username` varchar(20) NOT NULL,
      `password` varchar(70) NOT NULL,
      PRIMARY KEY  (`uid`)
    ) ENGINE=MyISAM  DEFAULT CHARSET=utf8 AUTO_INCREMENT=2 ;
    --
    -- 转存表中的数据 `tbl_user`
    --
    INSERT INTO `tbl_user` (`uid`, `username`, `password`) VALUES
    (1, 'syang', 'syang');
    -- --------------------------------------------------------
    --
    -- 表的结构 `tbl_userinfo`
    --
    CREATE TABLE IF NOT EXISTS `tbl_userinfo` (
      `infoid` int(11) NOT NULL auto_increment,
      `nickname` varchar(20) NOT NULL,
      `uid` int(11) NOT NULL,
      PRIMARY KEY  (`infoid`)
    ) ENGINE=MyISAM  DEFAULT CHARSET=utf8 AUTO_INCREMENT=2 ;
    --
    -- 转存表中的数据 `tbl_userinfo`
    --
    INSERT INTO `tbl_userinfo` (`infoid`, `nickname`, `uid`) VALUES
    (1, 'Song Yang', 1);

通过gii创建两个模型文件 User.php 和 Userinfo.php

修改User.php，添加关联代码

    public function relations()
    {
        return array(
            'info'=> array(self::HAS_ONE, 'Userinfo', 'uid')
        );
    }

下面，我们建一个新的控制器，名为TestController.php

    <?php
    /**
     * Description of TestController
     *
     * @author syang
     */
    class TestController extends Controller {
        public function actionIndex() {
            //AR数据提供
            $ar_provider = new CActiveDataProvider('User', array(
            ));
            //Sql数据提供
            $sql = "select * from tbl_user, tbl_userinfo where tbl_user.uid=tbl_userinfo.uid";
            $count = User::model()->findBySql($sql)->count();
            $sql_provider = new CSqlDataProvider($sql, array(
                'keyField'=>'uid',   //必须指定一个作为主键
                'totalItemCount'=>$count,    //分页必须指定总记录数
            ));
            //Array数据提供
            $sql = "select * from tbl_user, tbl_userinfo where tbl_user.uid=tbl_userinfo.uid";
            $array_data = Yii::app()->db->createCommand($sql)->queryAll();
            $array_provider = new CArrayDataProvider($array_data, array(
                'keyField'=>'uid',   //必须指定一个作为主键
            ));
            $this->render('index', array(
                            'ar_provider'=>$ar_provider,
                            'sql_provider'=>$sql_provider,
                            'array_provider'=>$array_provider
                        ));
        }
    }

下面，我们在view下建一个以控制器id命名的目录，如上为test，并在test目录下建立一个index.php的模块文件

    <?php
    echo "AR关联查询，示例:";
     $this->widget('zii.widgets.grid.CGridView', array(
        'id'=>'ar',
        'dataProvider'=>$ar_provider,
        'columns'=>array(
            'uid',
            array('header'=>'用户名', 'name'=>'username'),       //name对应字段名
            array('header'=>'密码', 'value'=>'$data->password'),  //用value这样写也可以
            'info.infoid',
            array('header'=>'昵称', 'name'=>'info.nickname'),
            //或者这样array('header'=>'昵称', 'value'=>'$data->info->nickname'),
            array(
                'class'=>'CButtonColumn',
            ),
        ),
    ));

    echo "Sql 查询，示例:";
    $this->widget('zii.widgets.grid.CGridView', array(
        'id'=>'sql',
        'dataProvider'=>$sql_provider,
        'columns'=>array(
            'uid',
            array('header'=>'用户名', 'name'=>'username'),
            array('header'=>'密码', 'value'=>'$data["password"]'),
            'infoid',
            array('header'=>'昵称', 'name'=>'nickname'),
            array(
                'class'=>'CButtonColumn',
                    'viewButtonUrl'=>'Yii::app()->controller->createUrl("view",array("id"=>$data["uid"]))',
                'updateButtonUrl'=>'Yii::app()->controller->createUrl("update",array("id"=>$data["uid"]))',
                'deleteButtonUrl'=>'Yii::app()->controller->createUrl("delete",array("id"=>$data["uid"]))',
            ),
        ),
    ));

    echo "Array 数据，示例:";
    $this->widget('zii.widgets.grid.CGridView', array(
        'id'=>'array',
        'dataProvider'=>$array_provider,
        'columns'=>array(
            'uid',
            array('header'=>'用户名', 'name'=>'username'),
            array('header'=>'密码', 'value'=>'$data["password"]'),
            'infoid',
            array('header'=>'昵称', 'name'=>'nickname'),
            array(
                'class'=>'CButtonColumn',
                    'viewButtonUrl'=>'Yii::app()->controller->createUrl("view",array("id"=>$data["uid"]))',
                'updateButtonUrl'=>'Yii::app()->controller->createUrl("update",array("id"=>$data["uid"]))',
                'deleteButtonUrl'=>'Yii::app()->controller->createUrl("delete",array("id"=>$data["uid"]))',
            ),
        ),
    ));

最终效果

总结:上面的示例演示了，使用CActiveDataProvider, CArrayDataProvider, CSqlDataProvider的例子

最简单的就是使用CActiveDataProvider，其他两个，在使用gridview挂件时，需要稍微修改一下，因为gridview原有button列上的连接使用的是$data->这种对象的方式

由于sql与array返回的数据都是数组，所以后面的地方改成数组就可以了。本例还演示了模型关联时使用CActiveDataProvider与gridview的列子。读者可以反复测试体会Yii的强大功能。

### model调用使用不同数据库

    // 使用其他数据库
    Team::$db = Yii::app()->db2;
    // 进行查询操作
    $team = Team::model()->findByPk($id);
    // 切换回到默认数据库
    Team::$db = Yii::app()->db;

### Yii 调试SQL

1系统自带调试
index.php开启调试模式

    // remove the following lines when in production mode
    defined('YII_DEBUG') or define('YII_DEBUG',true);
    // specify how many levels of call stack should be shown in each log message
    defined('YII_TRACE_LEVEL') or define('YII_TRACE_LEVEL',3);
    //app use time
    //defined('YII_BEGIN_TIME') or define('YII_BEGIN_TIME',microtime(true));

main.php

    'errorHandler'=>array(
        // use 'site/error' action to display errors
        'errorAction'=>'site/error',
    ),
    'log'=>array(
        'class'=>'CLogRouter',
        'routes'=>array(
            array(
                'class'=>'CFileLogRoute',
                'levels'=>'error, warning',
            ),
            // 下面显示页面日志
            array(
                'class'=>'CWebLogRoute',
                'levels'=>'trace',     //级别为trace
                'categories'=>'system.db.*' //只显示关于数据库信息,包括数据库连接,数据库执行语句
            ),
        ),
    ),

YII_TRACE_LEVEL的数字越大，信息越清楚

2调试工具

yii-debug-toolbar 把包解压后 放到extensions里边 然后在配置文件main.php中最后加上

    'log'=>array(
         'class'=>'CLogRouter',
         'routes'=>array(
              array(
                  'class'=>'ext.yii-debug-toolbar.YiiDebugToolbarRoute',
                  'ipFilters'=>array('127.0.0.1','192.168.1.215'),
              ),
         ),
     ),

没有出现的话加上在'components'下的db里加上两个属性，

    'enableProfiling'=>true,
    'enableParamLogging'=>true,

然后如果有其他调试工具的插件的话，可能会出现冲突sql语句不出来，把那段代码注掉

### Yii数据库迁移工具的使用

创建迁移目录：

    cd protected
    mkdir migrations

创建数据表：

    yiic migrate create create_user_table

修改迁移程序：

    vim protected\migrations\m110219_114900_create_user_table.php

    class m110219_114900_create_user_table extends CDbMigration {

        public function up() {
            $this->createTable('user',array(
                'id'=>'pk',
                'username'=>'varchar(60) NOT NULL',
                'password'=>'varchar(40) NOT NULL',
                'salt'=>'varchar(6) NOT NULL',
                'email'=>'varchar(120) NOT NULL',
                'created'=>'datetime NOT NULL',
                'last_access'=>'datetime NOT NULL',
                'last_login'=>'datetime NOT NULL',
                'status'=>'tinyint(1) NOT NULL default "0"',
            ));
        }

        public function down() {
            $this->dropTable('user');
        }

    }

    # 生成数据库表
    yiic migrate
    # 降低1个版本
    yiic migrate down 1
    # 升级1个版本
    yiic migrate up 1
    # 升级到指定版本
    yiic migrate to 101129_185401
    # 重新执行步骤
    yiic migrate redo [step]
    # 显示迁移历史
    yiic migrate history [limit]
    # 显示未应用的迁移
    yiic migrate new [limit]
    # 标记迁移历史到制定的版本，但是不实际执行
    yiic migrate mark 101129_185401

### Yii Framework的cookie使用方法

    设置cookie:
    $cookie = new CHttpCookie('mycookie','this is my cookie');
    $cookie->expire = time()+60*60*24*30;  //有限期30天
    Yii::app()->request->cookies['mycookie']=$cookie;

    读取cookie:
    $cookie = Yii::app()->request->getCookies();
    echo $cookie['mycookie']->value;

    销毁cookie:
    $cookie = Yii::app()->request->getCookies();
    unset($cookie[$name]);

### Yii中为表单添加必填字段域验证

关于我们新创建的项目(project)表单中所有字段域没有标记为必填，我们可以不输入任何数据就能提交成功。但是，我们知道，每个表单至少需要一个必填字段。让我们设置一个必填字段。为表单添加必填字段域
在Yii中当表单与AR模型类交互时，设置一个验证规则来限制字段域的范围。这需要在Project AR模型类的rules()方法中，添加一个数组，数组中包含特定的值。打开 /protected/models/Project.php类，已经看到了公共的rules方法被定义了，并且在rules方法中已经存在了一些规则：

    /**
    * @为model返回一个数组规则
    */
    public function rules()
    {
    return array(
      array('create_user_id, update_user_id', 'numerical', 'integerOnly'=>true),
      array('name', 'length', 'max'=>128),
      array('create_time, update_time', 'safe'),
      array('id, name, description, create_time, create_user_id, update_time, update_user_id', 'safe', 'on'=>'search'),
    );

rules()方法返回的是一个规则数组，一般每一个规则格式如下所示：
Array('Attribute List', 'Validator', 'on'=>'Scenario List', …additional options); Attribute List(属性列表)是一个字符串，需要验证的类的属性名用逗号分开。Validator(验证器)指的是使用什么样的规则执行验证。on这个参数指定了一个scenario(情景)列表来使用这条验证规则。 提示：scenario(情景)允许你限制验证规则应用在特定的上下文中。一种典型的例子是insert(插入)或update(更新）。例如：如果被指定为'on'=>'insert'，这将表明验证规则只适用于模型的插入情景。这同样适用于'update'或其它的任何你希望定义的情景。你可以设置一个模型的scenario(情景)属性或能过构造函数传给一个模型的实例。 如果这里没有设置，该规则将适用于调用save()方法的所有情景。最后，additional options(附加选项）是name/value(键值对)出现的用来初始化validator(验证器)的属性。 validator(验证器)可以是模型类中的一个方法或一个单独的验证器类。如果定义为模型类中的方法，它的格式必须是如下的形式：

    public function ValidatorName($attribute,$params) { ... }

如果我们使用一个validator(验证器)类，则这个类必须继承CValidator。其实有三种方法可以指定validator(验证器)，包括前面提到的一种格式：

1. 第一种是在模型类中定义验证方法
2. 第二种是指定一个单独的验证器类（这个类继承CValidator)。
3. 第三种是你可以使用Yii框架中现有的验证器，指定预定义的验证器别名即可。 Yii为你提供了很多预定义的验证器类，同时也指定了别名，用在定义规则时。Yii1.1版本，预定义的验证器别名的完整列表如下：
    * boolean：它是CBooleanValidator类的别名，验证属性的值是布尔值(true或false)。
    * captcha：它是CCaptchaValidator类的别名，验证属性的值等于一个显示的CAPTCHA(验证码)的值。
    * compare：它是CCompareValidator类的别名，验证属性的值与另一个属性的值相等。
    * email：它是CEmailValidator类的别名，验证属性的值为有一个有效的Email地址。
    * default：它是CDefaultValidator类的别名，验证属性的值为分配的默认值。
    * exist：它是CExistValidator类的别名，验证属性的值在表中的对应列中存在。
    * file：它是CFileValidator类的别名，验证属性的值包含上传的文件。
    * filter：它是CFilterValidator类的别名，用过滤器转换属性的值。
    * in：它是CRangeValidator类的别名，验证属性值在一个预定义列表中。
    * length：它是CStringValidator类的别名，验证属性值的长度在一个范围内。
    * match：它是CRegularExpressionValidator类的别名，验证属性值匹配一个正则表达式。
    * numerical：它是CNumberValidator类的别名，验证属性值是数字。
    * required：它是CRequiredValidator类的别名，验证属性值必需有值，不能为空。
    * type：它是CTypedValidator类的别名，验证属性值是一个指定的数据类型。
    * unique：它是CUniquedValidator类的别名，验证属性值在表中的对应列中是唯一的。
    * url：它是CUrlValidator类的别名，验证属性值是一个有效的URL。因为我们想使项目(project)的name属性字段必填，这个看着好像应该使用required别名可以满足我们的需要。让我们添加一验证规则指定name属性的验证器为required别名。我们将追加到现有的规则中：

代码

    public function rules()
    {
        return array(
            array('create_by',   'numerical','integerOnly'=>true),
            array('name', 'length', 'max'=>128),
            array('create_at, update_at', 'safe'),
            array('id, name, description, create_at, create_by,update_at, update_by', 'safe', 'on'=>'search'),
            array('name', 'required'),
        );
    }

### Yii扩展中通过widget方式使用Highcharts

插件地址: <http://www.yiiframework.com/extension/highcharts>

中文文档:

<http://www.helloweba.com/view-blog-156.html>

    $this ->Widget('ext.highcharts.HighchartsWidget', array(
       'options' => array(
           'title' => array('text' => '南京巾帼志愿者人数统计'),
           'xAxis' => array(
               'categories' => array(
                   '南京', '晓庄', '方山',
               ),
               'labels' => array(
                   'rotation' => -45,
                   'align' => 'right',
                   'style' => array(
                       'font' => 'normal 13px Verdana, sans-serif',
               )),
           ),
           'yAxis' => array(
               'min' => 0,
               'title' => array('text' => '人数')
           ),
           'legend' => array(
               'enabled' => false,
           ),
           'tooltip' => array(
               'formatter' => 'js:function() {
           return \'<strong>\'+ this.x +\'</strong>\'+
               \': \'+this.y +
               \'\';

     }' ),
           'series' => array(
               array(
                   'type' => 'column',
                   'data' => array(1, 5, 4),
                   'dataLabels' => array(
                       'enabled' => true,
                       'rotation' => -90,
                       'color' => '#FFFFFF',
                       'align' => 'center',
                       'x' => -3,
                       'y' => 10,
                       'formatter' => 'js:function() {
                        return this.y;
                        }',
                       'style' => array(
                           'font' => 'normal 13px Verdana, sans-serif',
                       ),
                   ),
               ),
           )
       )
    ));

### Yii中创建自己的Widget

下面以一个随机广告图片为例说明Yii中Widget的用法

1. 调用Widget

    <?php $this->widget('WidgetName'); ?>

或者

    <?php $widget=$this->beginWidget('path.to.WidgetClass'); ?>
    ...可能会由小物件获取的内容主体...
    <?php $this->endWidget(); ?>

也可以传参到Widget类

    <?php $userId = 1; ?>
    <?php $this->widget('WidgetName',array('userId'=>$userId)); ?>

参数userId自动映射到Widget类的同名属性，所以在定义Widget时，别忘记了声明该属性。

2. 创建Widget

自定义Widget类要继承CWidget,覆盖方法run

    <?php
    class BannerMagic extends CWidget {
        public function run(){
        }
    }

或者:

    class MyWidget extends CWidget {
        public function init() {
            // 此方法会被 CController::beginWidget() 调用
        }
         public function run() {
            // 此方法会被 CController::endWidget() 调用
        }
    }

下面是是BannerMagicWidget实现


    <?php class BannerMagicWidget extends CWidget {
       public function run() {
         $random = rand(1,3);
         if ($random == 1) {
           $advert = "advert1.jpg";
         }  else if ($random == 2) {
           $advert = "advert2.jpg";
         }  else {
           $advert = "advert3.jpg";
         }
         $this->render('bannermagic',array(
           "advert"=>$advert,
         ));
       }
    }

存储到protected\components\BannerMagicWidget.php

对应的view文件可能的内容如下：

    <img src="images/adverts/<?php echo $advert; ?>" alt="whatever" />

存储到protected\components\views\bannermagic.php

3. 调用该Widget

    <?php $this->widget('BannerMagicWidget'); ?>

### yii fckeditor

    <?php $this->widget('application.extensions.fckeditor.FCKEditorWidget',array(

        "model"=>$model,                # Data-Model
        "attribute"=>'group_id',         # Attribute in the Data-Model
        "height"=>'200px',
        "width"=>'100%',
        "toolbarSet"=>'xm_pulse',             # EXISTING(!) Toolbar (see: fckeditor.js)
        "fckeditor"=>Yii::app()->basePath."/../fckeditor/fckeditor.php",
                                        # Path to fckeditor.php
        "fckBasePath"=>Yii::app()->baseUrl."/fckeditor/",
    )); ?>

### Yii dropDownList 下拉菜单 联动菜单

视图

    <?php
    echo CHtml::dropDownList('country_id','', array(1=>'USA',7=>'France',3=>'Japan'),
    array(
        'ajax' => array(
        'type'=>'POST', //request type
        'url'=>Yii::app()->createUrl('project/dynamiccities'),
        'update'=>'#city_id', //selector to update
        'data'=>array(Yii::app()->request->csrfTokenName=>Yii::app()->request->getCsrfToken(),'country_id'=>'js $("#country_id").val()')
        //leave out the data key to pass all form values through
    )));

    //empty since it will be filled by the other dropdown
    echo CHtml::dropDownList('city_id','', array());

    ?>

控制器

    public function actionDynamiccities()
    {
        $data=Parts::model()->findAll('parent_id=:parent_id',
                      array(':parent_id'=>(int) $_POST['country_id']));

        $data=CHtml::listData($data,'id','name');
        foreach($data as $value=>$name)
        {
            echo CHtml::tag('option',
                       array('value'=>$value),CHtml::encode($name),true);
        }
    }

### yii安装rights

- 复制rights到modules
- 修改main.php

        'import'=>array(
             ......
             'application.modules.rights.*',
             'application.modules.rights.components.*', // Correct paths if necessary.
        ),
        ......
        'components'=>array(
             ......
             'user'=>array(
                  'class'=>'RWebUser', // Allows super users access implicitly.
                  ......
             ),
             'authManager'=>array(
                  'class'=>'RDbAuthManager', // Provides support authorization item sorting.
                  ......
             ),
             ......
        ),
        'modules'=>array(
             'rights'=>array(
                  'install'=>true, // Enables the installer.
             ),
        ),

- 修改controller

        class UserDataMonitoringController extends RController

        public function filters()
        {
            return array(
               'rights', // perform access control for CRUD operations
            );
        }

- 删除

         public function accessRules()
         ......

### 自定义 CGridView 中的操作按钮，关于 CButtonColumn 的应用

常见用法：

在CGridView中，使用CButtonColumn可以很方便地调用查看、更新、删除链接按钮：

    array(
      'header'=>'操作',
      'class'=>'CButtonColumn',
      'headerHtmlOptions'=>array(
          'width'=>'80',
      ),
      'template'=>'{view} {update} {delete}', //默认显示的三个按钮，要去掉某个按钮，删除相应项即可
    ),

自定义一个按钮：

譬如现在本人想添加一个额外的功能按钮“充值”，点击可跳转到该用户的充值页面。

首先，在template中加入新的标签：'template'=>'{recharge} {view} {update} {delete}',

然后，定义buttons，加入recharge按钮的定义：

    array(
        'header' => '操作',
        'class'=>'CButtonColumn',
        'headerHtmlOptions' => array('width'=>'90'),
        'htmlOptions' => array('align'=>'center'),
        'template'=>'{recharge} {delete}',
        'buttons'=>array(
            'recharge' => array(
                'label'=>'充值',
                'imageUrl'=>Yii::app()->request->baseUrl.'/images/icons/coins.png',
                'url'=>'Yii::app()->createUrl("member/recharge", array("id"=>$data->id))',
            ),
        ),
    ),

更多使用技巧：

单个按钮的属性：

    'buttonID' => array
    (
        'label'=>'...',     //显示为图片的title，鼠标放上去以后显示的文字
        'url'=>'...',       //超链接的地址
        'imageUrl'=>'...',  //按钮的图片src
        'options'=>array(), //按钮的html属性，譬如width,height,style,class等等
        'click'=>'...',     //点击的JS动作，需放在function(){}中
        'visible'=>'...',   //显示该按钮的条件，如 'visible'=>'$data->score > 0',
    )

修改删除按钮的提示：

    array (
        'class'=>'CButtonColumn',
        'deleteConfirmation'=>"js:'Record with ID '+$(this).parent().parent().children(':first-child').text()+' will be deleted! Continue?'",
    ),

//以上例子提示还读取了本行的第一列的值作为提示内容

参考文档：http://www.yiiframework.com/wiki/106/using-cbuttoncolumn-to-customize-buttons-in-cgridview/

### COutputCache实现整页缓存，文件缓存

1. 修改在config文件加入缓存组件

        'cache' => array (
            'class' => 'system.caching.CFileCache'
        ),

2. 在要做缓存的控制器里定义过滤器

        我现在要定义SiteController
        public function filters() {
                return array (
                    array (
                        'COutputCache + index, category, content',
                        'duration' => 3600,
                        'varyByParam' => array('id','page'),
                    )
                );
            }
        // COutputCache 是用于处理缓存的类，如果只填'COutputCache'，
        则该控制器里所有action都会进行缓存过滤，定义为'COutputCache+index,category,content'，
        表示只缓存actionIndex, actionCategory, actionContent
        // duration 是缓存的时间，单位是秒，上例中定义的3600即1小时
        // varyByParam 是指定一系列GET参数名称列表, 使用相应的值去确定缓存内容的版本，
        即同一个action用于区分是不同页面的的参数，此处我以id和page来区分不同页面，
        若我把page参数删掉，即写成 'varyByParam' => array('id'), 则以下两个页面采用同一缓存，导致翻页无效：
        //  index.php?r=site/index&id=2&page=1
        //  index.php?r=site/index&id=2&page=2
        // 除varyByParam以外，你还可以采用其他的条件来区分页面：
        // varyByExpression：指定是否缓存内容因承指定PHP表达式的结果而变化
        // varyByRoute：指定缓存内容基于请求的路由不同而变化 (controller 和 action)
        // varyBySession：指定是否缓存内容. 因用户session不同而变化

参考：http://www.yiiframework.com/doc/api/1.1/COutputCache
      http://www.yiichina.com/api/COutputCache

### 重写分页器 CLinkPager

1、自定义的分页器类放在哪里？

有两个位置可以放，

第一种是放在 protected/extensions 中，在使用是import进来，或在config文件中import进来；

第二种是放在 protected/components 中，作为组件存在，不需要import


2、用派生方式是最好的

    class MyPager extends CLinkPager

入口函数是：public function run() ，当显示分页器时run()被调用，里面的输出就会显示在相应位置；

其他的完全自定义，如果你不知道上一页、下一页、首页、尾页、总页数、当前页码等信息，可以参考CLinkPager的源码，yii/frameworks/web/widgets/pagers/CLinkPager.php

3、调用方式

在View里的相应widget，定义pager的class为自定义的分页器类名即可，参考：

    $this->widget('zii.widgets.CListView', array(
        'dataProvider'=>$dataProvider,
        'itemView'=>'_view_t',
        'pager'=>array(
      'class'=>'MyPager',
     )
    ));

### Yii 如何制作多语言版 网站

假设：你的程序源语言为英文，需要制作简体中文版。

1. 复制framework\messages\config.php 文件到 protected\messages\下

2. 更改config.php 'languages'=>array('zh_cn',)

3. 打开命令行工具 ,进入framework 目录 ,执行yiic message "..\protected\messages\config.php"

在protected\messages\下会生产zh_cn 目录，若发现为空，是因为你没有需要翻译的内容。
接下来：

4. 找到你需要翻译的视图文件 如：Yii 自带blog 例子 `post\_view.php` 中 `Tags:`
改为 `<? php echo Yii::t('post','Tags')` 再执行命令上述命令 就可以看到zh_cn 目录下post.php 文件了。翻译之。

5. 更改protected\config\main.php

    'language'=>'zh_cn',
    'sourceLanguage'=>'en_us',

完毕，即可看到效果。

### Yii框架的多语言设置

Yii 框架的缺省语言是美国英语（en_us ）。但是身在在中国，又希望致力于中国企业的信息化建设，所以让Yii 支持多语言（至少简体中文和美国英语）那是必须的。

1.全局语言

和其它application 级别的配置一样，系统的缺省语言可以在protected/config/main.php 中设置：

    return array(
         'basePath'=>dirname(__FILE__).DIRECTORY_SEPARATOR.'..',
         ......
         'language'=>'zh_cn', *** 不设置的话缺省为 en_us

2.Controller 级别

由于Yii 框架中Controller 掌控几乎全部的Views ，所以我们可以通过在Controller 的初始化函数中指定该Controller 控制的所有Views 的缺省语言。

    class foo extends Controller
    {
         public function init() {
              Yii::app()->language = ‘zh_cn’;
         }
         ……

需要动态切换语言的系统需要更多的逻辑。目前通行的做法是在页面的某个位置（多数是右上角）放置语言的链接，例如：中文 | English 。

    echo CHtml:: link ('中文', array('', 'hl'=>'zh')) . '|' .CHtml::link('English', array('', 'hl'=>'en'));

这样在点击相应的语言链接之后，利用Cache 将语言选择保存在服务器端。同样在Controller::init() 函数中根据Cache 中缓存的语言设置系统的缺省语言。

    public function init()
    {
         If (isset ($_GET['hl']) && 'en'===$_GET['hl']) {
              setcookie ("hl", 'en_us');
              $_COOKIE['hl']='en_us';   //cookie 不能立刻生效
         } else if (isset ($_GET['hl']) && 'zh'===$_GET['hl']) {
              unset ($_COOKIE['hl']);
              setcookie ("hl", "");
         }

         If ('en_us'==$_COOKIE['hl']) {
              Yii::app()->language='en_us';
         }
    }

3.文本翻译

为了解决文本的不同语言版本的动态选择，Yii 框架提供一个全局性的函数Yii::t(‘[text file]’, ‘[text]’) 用来封装所有需要多语言支持的文本。其中第一个参数’[text file]’ 代表存储当前语言文本的文件，第二个参数‘[text] ’是文本编码。’[text]’ 通常就是该文本在系统的缺省语言中的版本。例如：Yii 框架缺省的系统语言是美国英语，所以’[text]’ 通常就是文本的英文语意。但是如果在protected/config/main.php 中设置系统的缺省语言是简体中文，那么’[text]’ 应该是文本的简体中文语意。以文本“Name （名称）”为例，如果系统语言是美国英语，我们可以将’[text]’ 定义为’Name’ ；而当系统语言是简体中文时，’[text]’ 应该定义为’ 名称’ 。

和大多数多语言框架一样，Yii 框架也将不同语言的文本保存在该语言对应的目录下，作为一个资源。基于Yii 框架的系统在根目录下有一个messages 目录（[webapp]/messages ）。简体中文资源放置在[webapp]/messages/zh_cn 下，美国英语资源放置在[webapp]/messages/en_us 下。所有语言资源都是以PHP 文件的形式存在，且都返回一个包含若干Key/Value 对的数组。其中Key 就是Yii::t() 的第二参数’[text]’ 。仍以文本“Name （名称）”为例，如果系统语言是美国英语，语言资源文件中对应的Key/Value 应该是’Name’=>’ 名称’ ； 而当系统语言是简体中文时，Key/Value 应该是’ 名称’=>’Name’ 。值得注意的是，因为Yii 框架是使用UTF-8 编码，所以语言资源文件也必须是UTF-8 编码。否则显示文本时会出现乱码。

Yii 框架在运行时，会首先根据Yii::app()->language 的值定位到对应语言目录下的[text file].php 文件。然后再根据’[text]’ 在Key/Value 对数组中定位该’[text]’ 对应的语言文本，作为最终显示的文本。

### grid-view 自定义按钮 弹出窗口

对话框

    <?php
    //对话框
    $this->beginWidget('zii.widgets.jui.CJuiDialog', array(
        'id' => 'pay_confirm_dialog',
        'options' => array(
            'title' => '对话框',
            'autoOpen' => false,
            'minWidth' => 500,
            'height' => 300,
            'modal' => true,
            'resizable'=>false,
            'htmlOptions' => array(
                'style' => 'top:80px'
            )
        ),
    ));

    $this->endWidget('zii.widget.jui.CJuiDialog');
    ?>

自定义按钮

    array(
        'class'=>'CButtonColumn',
        'template' => '{confirm}',
        'buttons' => array(
            'confirm' => array(
                'label' => '确认',
                //'imageUrl' => '/images/icons/icon_approve.png',
                'url' => 'Yii::app()->controller->createUrl("post/create",array("id"=>$data->primaryKey))',
                //'click' => 'function(){return confirm("确定要通过这个文章吗？");}',
                'options' => array('class'=>'confirm'),
            ),
        ),
    )

ajax

    <script type="text/javascript">
    /*<![CDATA[*/
    jQuery(function($) {
    jQuery('#pay_confirm_grid a.confirm').live('click',function() {
         if(!confirm('确定要通过这个文章吗?')) return false;
         var th=this;
         var afterConfirm=function(){};
         $.fn.yiiGridView.update('user-grid', {
              type:'POST',
              url:$(this).attr('href'),
              success:function(data) {
                   $.fn.yiiGridView.update('user-grid');
                   afterConfirm(th,true,data);
              },
              error:function() {
                   afterConfirm(th,false);
              }
         });
         return false;
    });
    });
    /*]] > */
    </script>

弹出窗口的ajax

    <script type="text/javascript">
    /*<![CDATA[*/
    jQuery('body').undelegate('#yt0','click').delegate('#yt0','click',function(){
        jQuery.ajax({
            'type':'POST',
            'timeout':'30000',
            'data':{order_id:$("#orderid").val()},
            'beforeSend':function(){
                $("#yt0").attr('disabled','true');
                $("#load").show();
            },
            'success':function(html){
                $("#mydialog").html(html);
                $("#mydialog").dialog("open");
                setTimeout(function(){window.location.reload()},3000);
            },
            'complete':function(){
                $("#yt0").attr('disabled',false);
                $("#load").hide();
            },
            'error':function(a,b,c){
                if(b=="timeout") {
                    alert("本次执行过程超过30秒，请稍后重试！");
                }
            },
            'url':'/finance/refound/duppayinfo',
            'cache':false
        });
        return false;
    });
    /*]] > */
    </script>

### Send HTTP Post request with YII Framework

    <?php

    // URL on which we have to post data
    $url = "http://localhost/tutorials/post.php";

    // Any other field you might want to post
    $json_data = json_encode(array("name"=>"PHP Rockstart", "age"=>29));
    $post_data['json_data'] = $json_data;
    $post_data['secure_hash'] = mktime();

    // Initialize cURL
    $ch = curl_init();

    // Set URL on which you want to post the Form and/or data
    curl_setopt($ch, CURLOPT_URL, $url);
    // Data+Files to be posted
    curl_setopt($ch, CURLOPT_POSTFIELDS, $post_data);
    // Pass TRUE or 1 if you want to wait for and catch the response against the request made
    curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
    // For Debug mode; shows up any error encountered during the operation
    curl_setopt($ch, CURLOPT_VERBOSE, 1);
    // Execute the request
    $response = curl_exec($ch);

    // Just for debug: to see response
    echo $response;

### yii 使用 phpunit 3.7 以上版本

如果使用 PHPUnit 3.7 以上, 需要修改 yii/framework/test/CTestCase.php :

    require_once('PHPUnit/Runner/Version.php');
    // workaround for PHPUnit <= 3.6.11
    require_once('PHPUnit/Util/Filesystem.php');
    require_once('PHPUnit/Autoload.php');
    // PHPUnit >= 3.7
    if (in_array('phpunit_autoload', spl_autoload_functions())) {
        'phpunit_alutoload' was obsoleted
        spl_autoload_unregister('phpunit_autoload');
        Yii::registerAutoloader('phpunit_autoload');
    }

### Yiiframework框架之模块

模块是一个独立的单元，包含视图、控制器和其它组件，它和一个应用的区别是不能单独部署。

创建模块

首先，模块目录放在你的应用的modules目录中，我们可以使用yii自带的gii生成器来创建基本的结构，开启gii的方法是修改你的应用config/main.php文件中如下内容

    'modules'=>array(
        'gii'=>array(
            'class'=>'system.gii.GiiModule',
            'password'=>'你的密码访问时需要输入',
            // If removed, Gii defaults to localhost only. Edit carefully to taste.
            'ipFilters'=>array('127.0.0.1','::1'),
        ),
    ),

下面通过 你的应用/index.php?r=gii 访问gii，打开以后选择，左边菜单的Module Generator选项。你将会看到下面的画面

在Module ID 输入模块的名称，我这里输入admin,然后点击Preview按钮。如下图所示，它向你展示了所有将会被生成的文件，允许你在新建之前预览他们 :

然后点击Generate按钮，来生成所有文件。因为Web服务器进程需要写入权限，所以确保你的/protected文件夹对于该应用程序是可写入的。

配置使用这个模块

我们对主配置文件 : protected/config/main.php进行配置，如下的代码需要被修改,添加了'admin', :

    'modules'=>array(
         'gii'=>array(
         'class'=>'system.gii.GiiModule',
         'password'=>'你的密码',
         ),
         'admin',
    ),

保存上面的修改后后，我们的新admin模块已经可以使用了。我们可以先通过以下地址访问一下 `index.php?r=admin/default/index`

在模块中使用layout

我们访问index.php?r=admin/default/index会发现，模块使用了你的应用下的/protected/views /layouts/main.php文件，而我们可能希望使用/protected/modules/admin/views/layouts /main.php文件，让admin模块拥有独立的布局视图。
我们在protected\modules\admin\controllers\DefaultController.php添加如下代码，

    public $layout='application.modules.admin.views.layouts.main';

我们把从/protected/views/layouts/main.php 拷贝到/protected/modules/admin/views/layouts/，稍作修改，这样模块就拥有了独立的布局视图.

在模块中使用Assets

添加新的模块时，一般会包含图像文件，CSS文件，JavaScript文件等。
模块可以直接从网站主目录中引用。但是如果想要创建一个模块能够在任何地方引用，并且能够避免命名冲突，就要用到assets了。

过程是(这里模块名是admin）：

1. 把需要用到的资源放在modules/admin/assets下。
2. 然后通过 CAssetManager，Yii::app()->assetManager能够自动的将私有资源publish到公共目录下 网站目录/assets
3. Yii会自动在网站目录的/assets下创建一个随机不冲突的文件夹,如2b31b42b，并把你的modules/admin/assets目录下的文件拷贝过去。

    例如我的模块是Admin，文件路径通过如下代码获得，修改protected\modules\admin\AdminModule.php文件，

        class AdminModule extends CWebModule
        {
            private$_assetsUrl;

            publicfunction getAssetsUrl()
            {
                if($this->_assetsUrl===null)
                $this->_assetsUrl=Yii::app()->getAssetManager()
                    ->publish(Yii::getPathOfAlias('application.modules.admin.assets'));
                return$this->_assetsUrl;
            }

            publicfunction setAssetsUrl($value)
            {
                $this->_assetsUrl=$value;
            }
        }

    然后，在/protected/modules/admin/views/layouts/main.php中
    使用$this->module->assetsUrl就可以调用你的css等文件了。

        <link rel="stylesheet" type="text/css"
            href="<?php echo $this->module->assetsUrl; ?>/css/screen.css" media="screen, projection" />

4. 通过如上操作，该模块只要把admin目录拷贝，就可以多次复用了。

### 关于yii的relations

yii的relations里self::BELONGS_TO默认是用当前指定的键跟关联表的主键进行join，例如：

    return array(
         'reply' => array(self::BELONGS_TO, 'BookPostReply', 'postid'),
    );

默认生成的sql是 on id = postid，id是BookPostReply的主键。
但今天我遇到的需求却是需要生成 on BookPostReply.postid = t.postid，不去关联主键，而且关联其中一个字段的值，怎么搞都搞不定，论坛也翻了个遍，不得不说，yii的论坛搜索功能真的很烂，每次用都要从一 堆的内容里过滤信息，还不见的能找到自己想要的，而且手册也比较简单，对这些东西没有做比较深入的解答。

后来无意中看到有个on的属性，刚好跟sql里的on一样，于是抱着试试的想法，在配置里加了上去

    return array(
         'reply' => array(self::BELONGS_TO, 'BookPostReply', 'postid', 'on' => 't.postid=reply.postid'),
    );

看调试信息里的SQL语句，发现yii生成了一条 on id = postid and t.postid=reply.postid 这样的语句，看到这就已经明白这个东西的作用了，于是将postid清空，改成如下：

    return array(
         'reply' => array(self::BELONGS_TO, 'BookPostReply', '', 'on' => 't.postid=reply.postid'),
    );

终于将默认的on重置掉了^_^，yii确实很灵活，可惜一直没有一份详细的开发手册，导致很多时候要走不少弯路，希望以后手册能够更加完善起来吧！

model 里面 的$criteria->with = 'reply';
(可以先试试不加是否可以)
查找的时候必须使用'with'关键字，例如

    Member::model()->with('shop')->findAll($criteria);

如果查询中使用了字段名,需要修改为类似t.id

其他的类似代码

    'member' => array(self::BELONGS_TO,'ShopMember','member_id',
        'with'=>'extends','condition'=>"member.member_state = '1'"),
    'shop' => array(self::BELONGS_TO , 'ShopInfo' , '','on'=>'t.member_id = shop.member_id' ),

view使用的地方

    '$data->reply->title'

### Yii框架中ActiveRecord使用Relations

== 前提条件

在组织数据库时，需要使用主键与外键约束才能使用ActiveReocrd的关系操作；

== 场景

== 申明关系
两张表之间的关系无非三种：一对多；一对一；多对多； 在AR中，定义了四种关系：

    关系                    定义                                                                                 例子
    BELONGS_TO          A和B的关系是一对多，那么B属于A                                                        Post属于User
    HAS_MANY           A和B之间的关系是一对多，那么A有多个B                                              User有多个Post
    HAS_ONE           这是HAS_MANY的一种特殊情况，A至多有一个B                                         User至多有一个Profile
    MANY_MANY           这个对应多对多的情况，在AR里会将多对多以BELONGS_TO和HAS_MANY的组合来解释      Post和Category

在AR中通过重写CActiveRecord类的relations()方法来申明关系；这个方法返回一个关系配置的数组；一个数组无素代表一个单独的关系，格式如下：

    'VarName'=>array('RelationType','ClassName','ForeignKey', ...additional options)

    Var Name           关系名
    Relation Type      四种关系：self::BELONGS_TO, self::HAS_ONE, self::HAS_MANY, self::MANY_MANY
    Class Name           代表当前AR类要关联的那个AR类名
    Foreign Key      实现关系的外键， 有可能有多个，即列名

下面的代码表示用来定义Post, User之间的关系

    class Post extends CActiveRecord
    {
        public function relations()
        {
            return array('author' => array(self::BELONGS_TO, 'User', 'author_id'), 'categories' => array(self::MANY_MANY, 'Category', 'tbl_post_category(post_id, category_id)'));
        }
    }

    class User extends CActiveRecord
    {
        public function relations()
        {
            return array('posts' => array(self::HAS_MANY, 'Post', 'author_id'), 'profile' => array(self::HAS_ONE, 'Profile', 'owner_id'));
        }

    }


使用时，如果$author代表一个USER的AR实例，可以使用$author->posts来获取到它相关的所有的Post对象。


== 执行关系查询

=== 懒惰导入查询方法

最简单的方法就是为AR对象添加一个关联属性，
例:
// 获取PK为10的POST对象 $post=Post::model()->findByPk(10); // 获取这个POST的作者 $author=$post->author;
如果没有关联的对象，那么将返回NULL或者一个空数组；BELONGS_TO和HAS_ONE结果为NULL，而HAS_MANY和MANY_MANY返回一个空数组。
上面的这种“懒惰导入”方法使用起来非常方便，但是在一些场景下不是非常的效率，比如，如果我们想访问N个POST的作者的信息，使用这种懒惰导入的方法将会执行N个join查询；

=== 急切导入查询方法

下面介绍是一种“急切导入”方法：在使用find和findAll时，使用with()方法，例：

$posts=Post::model()->with('author')->findAll()

这样就可以在一次查询时连同查询其他信息了；with方法可以接受多个关系：

$posts=Post::model()->with('author','categories')->findAll();

这样就可以将作者和类别的信息一并进行查询；同样，with还支持多重急切导入

$posts=Post::model()->with( 'author.profile', 'author.posts', 'categories')->findAll();
上面的代码不仅会返回autho和categories信息，还会返回作者的profile和posts信息
这种“急切导入”方法也支持CDbCriteria::with，下面这两种实现方式效果一样：

    $criteria=new CDbCriteria;
    $criteria->with=array( 'author.profile', 'author.posts', 'categories', );
    $posts=Post::model()->findAll($criteria);
    $posts=Post::model()->findAll(array( 'with'=>array( 'author.profile', 'author.posts', 'categories' ));

== 关系查询选项

前面提过，在申明关系时可以添加额外的选项，这些选项都是一些key-value对，是用来定制关系查询的，总结如下：

    select
         定义从AR类中被select的列集合，如果定义为*，则表示查询所有列
    condition
         定义where语句，默认为空。
    params
         生成SQL语句的参数，这个需要用一个key-value对的数组来表示；
    on
         ON语句，这个条件用来通过AND添加一个joining condintion语句
    order
         ORDER语句
    with
         和当前对象一起导出的相关对象列表，要注意如果使用不正确，有可能导致无限死循环；
    joinType
         定义join的类别，默认为LEFT OUTER JOIN
    alias
         定义别名，当多个表中有相同的column name时，需要为表格定义alias，然后使用tablename.columnname来指定不同的column
    together
         这个只在HAS_MANY, MANY_MANY时有用，在实现跨表查询时，可以用这个参数来控制性能。正常用不到，不详细讲述；
    join
         JOIN语句
    group
         GROUP语句
    having
         HAVING语句
    index
         这个值用来设定返回的结果数组以哪个column做为index值，如果不设定这个值的话，将从0开始组织结果数组。

除此之外还包含下面几个选项，在“懒惰导出”的特定关系时可用

    limit
         返回结果数量的限制，不适用于BELONG_TO关系
    offset
         offset结果数量的值，不适用于BELONG_TO关系

下面代码，显示上面选项的一些使用：

    class User extends CActiveRecord
    {
        public function relations()
        {
            return array('posts' => array(self::HAS_MANY, 'Post', 'author_id', 'order' => 'posts.create_time DESC', 'with' => 'categories'), 'profile' => array(self::HAS_ONE, 'Profile', 'owner_id'));
        }
    }

此时，我们使用$author->posts时，会返回固定ORDER的POST信息

### Relation中非主键字段关联

有两张表，设计结构如下：

商品表：字段：p_id,member_id

商铺表：字段：shop_id,member_id

其中p_id,shop_id都是主键，member_id是索引

考虑过这个原因，是最初设计的时候，每一个普通用户都可以发布商品，所以商品表没有记录shop_id。

可能也会考虑过，每一个用户会有多个商铺吧，所以shop表的member_id也不是唯一索引。

然后，我用Yii在做表关联，事实上，在我们改动程序的时候，我们已经把普通用户能够发布商品这个功能去掉了，因此，事实上对我们来说member_id，其实肯定是于shop表中的member_id有对应关系。

于是我在ShopProduct的relations这样写

    /**
    * @return array relational rules.
    */
    public function relations()
    {
    // NOTE: you may need to adjust the relation name and the related
    // class name for the relations automatically generated below.
    return array(
    'member' => array(self::BELONGS_TO,'ShopMember','member_id','with'=>'extends','condition'=>"member.member_state = '1'"),
    'shop' => array(self::BELONGS_TO , 'ShopInfo' , 'member_id','on'=>'t.member_id = shop.member_id' ),
    );
    }

是的，看上去好象没问题，但是实际却关联不上。

在Yii的代码中，关于relation的on是这样写的：

    'on': the ON clause. The condition specified here will be appended to the joining condition using the AND operator. This option has been available since version 1.0.2.

所以，上述的代码在SQL中显示出来就是 select * from product left outer join shop on (product.member_id = shop.shop_id and product.member_id = shop.member_id)//这个代码是伪代码，只是为了说明问题。

出现这种情况可以将foreignKey字段设为空，然后再指定ON。

    /**
    * @return array relational rules.
    */
    public function relations()
    {
    // NOTE: you may need to adjust the relation name and the related
    // class name for the relations automatically generated below.
    return array(
        'member' => array(self::BELONGS_TO,'ShopMember','member_id','with'=>'extends','condition'=>"member.member_state = '1'"),
        'shop' => array(self::BELONGS_TO , 'ShopInfo' , '','on'=>'t.member_id = shop.member_id' ),
    );
    }

查找的时候必须使用'with'关键字，例如

    Member::model()->with('shop')->findAll($criteria);


### findByAttribute using condition and params

1. Supply a $condition as string:

    Person::model()->findByAttributes(
        array('first_name'=>$firstName,'last_name'=>$lastName),
        'status=1'
    );

2. Supply a $condition as string that contains placeholder and $params as array with placeholder values:

    Person::model()->findByAttributes(
        array('first_name'=>$firstName,'last_name'=>$lastName),
        'status=:status',
        array(':status'=>1)
    );

3. Supply a $condition as a CDbCriteria:

    $criteria=New CDbCritieria;
    $critieria->condition='status=1';
    Person::model()->findByAttributes(
        array('first_name'=>$firstName,'last_name'=>$lastName),
        $criteria
    );

4. Supply a $condition as array with property values for CDbCriteria:

    Person::model()->findByAttributes(
        array('first_name'=>$firstName,'last_name'=>$lastName),
        array(
            'condition'=>'status=:status',
            'params'=>array(':status'=>1)
        )
    );

### YII assets使用

为什么用YII assets

1.assets的作用是方便模块化，插件化的

一般来说出于安全原因不允许通过url访问protected下面的文件 ，但是我们又希望将module单独出来，所以需要使用发布，即将一个目录下的文件复制一份到assets下面方便通过url访问

    $assets = Yii::getPathOfAlias('ext').'/css';
    //$baseUrl = Yii::app()->getAssetManager()->publish($assets);
    $baseUrl = Yii::app()->assetManager->publish($assets);  //extensions/css发布到assets的创建一个随机不冲突的文件夹下
    Yii::app()->clientScript->registerCssFile($baseUrl.'/main.css');//引用assets下面的main.css

2.如果一个模块需要添加使用资源，直接从webroot中引用添加即可。

但是试图创建一个模块能够在任何地方引用，且资源独立并能够避免命名冲突 。
你如何保证你的文件名不会与一些零散的应用程序的尝试使用相同名称的文件冲突，对于js,images,css也一样。
通过CAssetManager，Yii::app()->assetManager能够自动的将私有资源publish到公共目录下webroot/assets

下面以admin module为例

1. 把需要用到的资源放在modules/admin/assets下。
2. 然后通过 CAssetManager，Yii::app()->assetManager能够自动的将私有资源publish到公共目录下 网站目录/assets
3. Yii会自动在网站目录的/assets下创建一个随机不冲突的文件夹,如2b31b42b，并把你的modules/admin/assets目录下的文件拷贝过去。

通过如下代码获得，修改protected\modules\admin\AdminModule.php文件，

    <?php
    class AdminModule extends CWebModule
    {
        private $_assetsUrl;

        public function getAssetsUrl()
        {
            if($this->_assetsUrl===null)
                $this->_assetsUrl=Yii::app()->getAssetManager()->publish(Yii::getPathOfAlias('application.modules.admin.assets'));
            return $this->_assetsUrl;
        }

        public function setAssetsUrl($value)
        {
            $this->_assetsUrl=$value;
        }
    }

然后，在/protected/modules/admin/views/layouts/main.php中

使用$this->module->assetsUrl就可以调用你的css等文件了。

    <link rel="stylesheet" type="text/css" href="<?php echo $this->module->assetsUrl; ?>/css/screen.css"/>

使用前强制更新asset

    $baseJsUrl = Yii::app()->getAssetManager()->publish($baseJsPath, false, -1, YII_DEBUG);

### yii 表结构缓存

    Yii::app()->cache0->flush();

    // 清除所有的表结构缓存
    Yii::app()->db->schema->refresh();
    // 加载应用中的所有表结构
    Yii::app()->db->schema->getTables();
    // 加载单个表结构
    Yii::app()->db->schema->getTable('tablename', true);

### model 验证

    $item->validate();
    $err = $item->getErrors();
    var_dump($err);
    exit;

### Other