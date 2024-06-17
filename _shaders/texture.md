---
title: "Texture"
excerpt: "UV mapping basic texture"
date: "2024-05-13"
classes: wide
sidebar:
  - title: "Shader Type"
    text: "HLSL"
  - title: "Additional tags"
    text: "Texture"
header:
  teaser: /assets/images/shaders/hlsl/texture.png
gallery:
---              

Basic shader for unpacking a texture and using a tint to modify it's color.

Example:
![Shader](../../assets/images/shaders/hlsl/texture.png)


Shader code:
```glsl
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "Unlit/shader_01_simple"
{
    Properties
    {
        _Tint ("Tint", Color) = (1.,1.,1.,1.)
        _MainTex("MainTex", 2D) = "White" {}
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
            sampler2D _MainTex;
            float4 _MainTex_ST;

            struct Interpolators
            {
                float4 position : SV_POSITION;
                float2 uv : TEXCOORD0;
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
                
                return i;
            }

            float4 frag(Interpolators i) : SV_TARGET
            {                
                //return float4(i.uv, 1, 1) * _Tint;
                return tex2D(_MainTex, i.uv) * _Tint;
            }
            
            ENDCG
        }        
    }
}
```