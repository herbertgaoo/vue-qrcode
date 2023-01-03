<template>
    <div class="qr-code">
        <qriously ref="qriously" :value="value" :size="size" level="H"/>
        <div slot="footer">
            <Button size="large" type="primary" @click="downloadQR">保存至本地</Button>
        </div>
    </div>
</template>
<script>
export default {
    name: "QrCode",
    props: {
        value: {type: String, default: ""},
        title: {type: String, default: ""},
        desc: {type: String, default: ""},
        logo: {type: String, default: ""},
        size: {type: Number, default: 200},
        gap: {type: Number, default: 0}
    },
    data() {
        return {
            padding: this.size / 10,
            cav: null,
            ctx: null
        };
    },
    computed: {
        show: {
            get() {
                return this.value;
            },
            set(val) {
                this.$emit("input", val)
            }
        }
    },
    methods: {
       downloadQR () {
        let link = document.createElement('a')
        link.href = this.$refs.qriously.$refs.qrcode.toDataURL('image/png')
        link.download = `${this.title}.png`
        link.target = '_blank'
        link.click()
       },
       drawImage(context, image, x, y, width, height, radius = 4) {
            context.shadowOffsetX = 0;
            context.shadowOffsetY = 2;
            context.shadowBlur = 4;
            context.shadowColor = '#00000040';
            context.lineWidth = 8;
            context.beginPath();
            context.moveTo(x + radius, y);
            context.arcTo(x + width, y, x + width, y + height, radius);
            context.arcTo(x + width, y + height, x, y + height, radius);
            context.arcTo(x, y + height, x, y, radius);
            context.arcTo(x, y, x + width, y, radius);
            context.closePath();
            context.strokeStyle = '#fff';
            context.stroke();
            context.clip();
            context.fillStyle = '#fff';
            context.fillRect(x, y, width, height);
            context.drawImage(image, x, y, width, height);
        }
    },
    created() {

    },
    mounted() {
        this.cav = this.$refs.qriously.$refs.qrcode
        this.ctx = this.cav.getContext("2d")
        let imgData = this.ctx.getImageData(0,0, this.cav.width, this.cav.height)

        if(this.gap !== 0) {this.padding = this.gap}

        this.cav.height = this.size + this.padding * 5
        this.cav.width = this.size + this.padding * 2

        if(!this.desc) {
            this.cav.height -= this.padding * 1.5
        }

        // 设置二维码背景色
        this.ctx.fillStyle = "#FFFFFF";
        this.ctx.fillRect(0, 0, this.size+this.padding*2, this.size+this.padding*2.5);

        // 当该像素是透明的,则设置成白色
        for(var i = 0; i < imgData.data.length; i += 4) {
            if(imgData.data[i + 3] == 0) {
                imgData.data[i] = 255;
                imgData.data[i + 1] = 255;
                imgData.data[i + 2] = 255;
                imgData.data[i + 3] = 255;
            }
        }
        this.ctx.putImageData(imgData, this.padding, this.padding*2.5)

        // 设置文本背景色
        this.ctx.fillRect(0, this.size+this.padding*2.5, this.size+this.padding*2, this.size);
        
        // 标题
        this.ctx.fillStyle = "#000000";
        this.ctx.textAlign = "center"
        this.ctx.font = `bold 20px 'Microsoft YaHei','PingFang SC'`
        this.ctx.fillText(this.title, this.cav.width/2, this.padding*1.5, this.size)

        // 描述
        if(this.desc) {
            this.ctx.fillStyle = "#666666";
            this.ctx.textAlign = "center"
            this.ctx.font = `14px 'Microsoft YaHei','PingFang SC'`
            this.ctx.fillText(this.desc, this.cav.width/2, this.size + this.padding*4, this.size)   
        }

        // logo
        if(this.logo) {
            const image = new Image();
            image.src = this.logo;
            image.crossorigin = 'anonymous';
            image.onload = () => {
                // const coverage = 0.15;
                // const width = Math.round(this.cav.width * coverage);
                const width = 32;
                const x = (this.cav.width - width) / 2;
    
                this.drawImage(this.ctx, image, x, x+this.padding*1.5, width, width);
            };
        }
    }
}
</script>
<style scoped lang="less">
.qr-code {
    .text-center;
    @{deep} canvas { box-shadow: 0 6px 16px 0 #00000033; }
    @{deep} .ivu-btn { .mgAll(30px 0 40px) }
}
</style>