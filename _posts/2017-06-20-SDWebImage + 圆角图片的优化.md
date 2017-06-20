---
layout:     post
title:      "SDWebImage + 圆角图片的优化"
subtitle:   ""
date:       2017-06-20 17:45:44 +0800
author:     "Liyf"
header-img: "img/common.png"
catalog:    true
tags: 
    - iOS
---

> 使用情景：从网络请求非圆角图片，在视图显示圆角，需要使用缓存

---  

#### 圆角图片
为 <code>UIImage</code> 添加类目，使用 <code>CoreGraphics</code> 绘制圆角图片
    - (UIImage *)drawCornerImage:(CGFloat)radius sizeToFit:(CGSize)size {
        CGRect rect = CGRectMake(0, 0, size.width, size.height);
    
        //  圆角路径，定义不同的路径实现三角形、椭圆等
        UIBezierPath *cornerPath = [UIBezierPath bezierPathWithRoundedRect:rect byRoundingCorners:UIRectCornerAllCorners cornerRadii:CGSizeMake(radius, radius)];
    
        UIGraphicsBeginImageContextWithOptions(size, NO, [UIScreen mainScreen].scale);
        CGContextAddPath(UIGraphicsGetCurrentContext(), cornerPath.CGPath);
        CGContextClip(UIGraphicsGetCurrentContext());
        [self drawInRect:rect];
    
        CGContextDrawPath(UIGraphicsGetCurrentContext(), kCGPathFillStroke);
        UIImage *output = UIGraphicsGetImageFromCurrentImageContext();
        UIGraphicsEndImageContext();
    
        return output;
    }

---

#### 网络圆角图片
> 1. 加载网络图片前使用 <code>SDImageCache</code> 中 <code>queryCacheOperationForKey: done:</code> 方法查询是否有使用 <code>SDWebImage</code> 缓存过该图片，如果有直接使用
2. 如果本地没有该图片，使用 <code>sd_setImageWithURL: placeholderImage: options: completed:</code> 方法请求图片，同时设置 <code>options</code> 参数为 <code>SDWebImageCacheMemoryOnly | SDWebImageAvoidAutoSetImage</code>
3. <code>SDWebImage</code> 请求图片后将该图片转为圆角，同时替换缓存中的图片

为<code>UIImageView</code> 添加类目，实现以下方法

    - (void)addCorner:(CGFloat)radius imageUrl:(NSString *)urlStr withPlacholderImage:(NSString *)placholderImage {
        __weak __typeof(&*self) weakSelf = self;
        [[SDImageCache sharedImageCache] queryCacheOperationForKey:urlStr done:^(UIImage * _Nullable image, NSData * _Nullable data, SDImageCacheType cacheType) {
            if (image != nil) {
                weakSelf.image = image;
            }
            else {
                [weakSelf sd_setImageWithURL:[NSURL URLWithString:urlStr] placeholderImage:[UIImage imageNamed:placholderImage] options:SDWebImageCacheMemoryOnly|SDWebImageAvoidAutoSetImage completed:^(UIImage * _Nullable image, NSError * _Nullable error, SDImageCacheType cacheType, NSURL * _Nullable imageURL) {
                    if (image != nil) {
                        UIImage *output = [image drawCornerImage:radius sizeToFit:weakSelf.bounds.size];
                    
                        weakSelf.image = output;
                        [[SDImageCache sharedImageCache] storeImage:output forKey:urlStr completion:^{
                        
                        }];
                    }
                }];
            }
        }];
    }

> 重新绘制圆角图片时，设置图片的大小为 <code>imageView</code> 的大小，可以避免图片适应控件大小的缩放，对性能有一定优化作用（这里不确定是改用 <code>imageView.bounds.size</code> 还是 <code>imageView.bounds.size * [UIScreen mainScreen].scale</code>）

---
#### ...
> - 其他，为 <code>UIButton</code> 等其他控制设置网络圆角图片可以同样封装
- 不足，从网络获取图片只能通过这个方法，如果本地已经存在没有改为圆角的同地址图片，将显示非圆角图片，这里需要改进