---
title: "Details texture"
excerpt: "Adding two textures together"
date: "2024-05-15"
classes: wide
sidebar:
  title: "Shader Type"
  text: "HLSL"
  title: "Additional tags"
  text: "details"
header:
  teaser: /assets/images/shaders/hlsl/details.png
gallery:
---              

Expansion on texture shader to allow for detail texture to be defined and properly blend it with the main texture.

![Shader](../../assets/images/shaders/hlsl/details.png)



CustomLight.cginc
```glsl
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "Custom/Textured With Detail"
{
    Properties
    {
        _Tint ("Tint", Color) = (1.,1.,1.,1.)
        _MainTex("MainTex", 2D) = "white" {}
        _DetailTex("DetailTex", 2D) = "gray" {}
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
            sampler2D _MainTex, _DetailTex;
            float4 _MainTex_ST, _DetailTex_ST;

            struct Interpolators
            {
                float4 position : SV_POSITION;
                float2 uv : TEXCOORD0;
                float2 uvDetail : TEXCOORD1;
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
                i.uvDetail = TRANSFORM_TEX(v.uv, _DetailTex);
                
                return i;
            }

            float4 frag(Interpolators i) : SV_TARGET
            {
                float4 color = tex2D(_MainTex, i.uv) * _Tint;
                color *= tex2D(_DetailTex, i.uvDetail) * unity_ColorSpaceDouble;
                return color;
                //return float4(i.uv, 1, 1) * _Tint;
            }
            
            ENDCG
        }        
    }
}
```