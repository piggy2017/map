<template>
    <div class="address">
      addressaddress
    </div>
</template>
<script>
  import BMap from "BMap";
    export default {
      data(){
        return{

        }
      },
      methods:{
        ready () {
          let map = new BMap.Map('allmap')
          let point = new BMap.Point(this.center.lng, this.center.lat)
          map.centerAndZoom(point, 18)  // 初始化地图，设置中心点坐标和地图级别
          map.enableScrollWheelZoom(true)   //开启鼠标滚轮缩放
          map.enableDoubleClickZoom(true)  //开启双击放大
          //let sContent="haha"
          var geolocation = new BMap.Geolocation()
          geolocation.getCurrentPosition((r) => {
            console.log(r);
            if (r.point) {
              this.center.lng = r.longitude
              this.center.lat = r.latitude
              let markers = new BMap.Marker(r.point)
              map.addOverlay(markers)
              map.panTo(r.point)
              map.centerAndZoom(r.point, 16)

              var point = new BMap.Point(this.center.lng,this.center.lat);//用所定位的经纬度查找所在地省市街道等信息
              var gc = new BMap.Geocoder();
              gc.getLocation(point, (rs)=>{
                console.log(rs);
                var addComp = rs.addressComponents;
                console.log(rs.address);//地址信息
                console.log(addComp.province+addComp.city+addComp.district+addComp.street+ addComp.streetNumber)
                //alert(rs.address);//弹出所在地址
                this.address=rs.address
              });

//              var infoWindow = new BMap.InfoWindow(sContent);  // 创建信息窗口对象
//              map.openInfoWindow(infoWindow,point); //开启信息窗口
//              document.getElementById("r-result").innerHTML = "信息窗口的内容是：<br />" + infoWindow.getContent();
            }
          }, { enableHighAccuracy: true })
        }
      },
      mounted () {
        this.ready()
      }
    }
</script>
<style>

</style>
