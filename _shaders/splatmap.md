---
title: "Splatmap"
excerpt: "Blending few textures based on rgb values."
date: "2024-05-18"
classes: wide
sidebar:
  - title: "Shader Type"
    text: "HLSL"
  - title: "Additional tags"
    text: "splatmap, blending textures based on rgb"
header:
  teaser: /assets/images/shaders/hlsl/splatmap.png
gallery:
---              

In this shader I've expanded on texture knowledge by blending final color of frag between 4 different textures, based on rgb of splatmap rgb channels.

Shader in action:
![Shader](../../assets/images/shaders/hlsl/splatmap.png)



Shader code
```glsl
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "Custom/Textured Splatting"
{
    Properties
    {
        _MainTex("MainTex", 2D) = "white" {}
        [NoScaleOffset] _Texture1("Texture 1", 2D) = "white" {}
        [NoScaleOffset] _Texture2("Texture 2", 2D) = "white" {}
        [NoScaleOffset] _Texture3("Texture 3", 2D) = "white" {}
        [NoScaleOffset] _Texture4("Texture 4", 2D) = "white" {}
    }    
    
    SubShader
    {
        Pass
        {
            CGPROGRAM

            #pragma vertex vert
            #pragma fragment frag

            #include "UnityCG.cginc"

            float4 _Tint;
            sampler2D _MainTex, _Texture1, _Texture2, _Texture3, _Texture4;
            float4 _MainTex_ST;

            struct Interpolators
            {
                float4 position : SV_POSITION;
                float2 uv : TEXCOORD0;
                float2 uvSplat : TEXCOORD1;
            };

            struct VertexData
            {
                float4 position : POSITION;
                float2 uv : TEXCOORD0;
            };

            Interpolators vert(VertexData v)
            {
                Interpolators i;
                i.uv = TRANSFORM_TEX(v.uv, _MainTex);
                i.position = UnityObjectToClipPos(v.position);
                i.uvSplat = v.uv;                
                
                return i;
            }

            float4 frag(Interpolators i) : SV_TARGET
            {
                float4 splat = tex2D(_MainTex, i.uvSplat);
                	return
					tex2D(_Texture1, i.uv) * splat.r +
					tex2D(_Texture2, i.uv) * splat.g +
					tex2D(_Texture3, i.uv) * splat.b +
					tex2D(_Texture4, i.uv) * (1 - splat.r - splat.g - splat.b);
            }
            
            ENDCG
        }        
    }
}
```