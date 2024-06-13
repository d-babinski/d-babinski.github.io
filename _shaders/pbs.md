---
title: "Physical Based Shading"
excerpt: "Implementing various BRDFs"
date: "2024-05-19"
classes: wide
sidebar:
  - title: "Shader Type"
    text: "HLSL"
  - title: "Additional tags"
    text: "pbs, physical based shading, Blinn=Phong model"
header:
  teaser: /assets/images/shaders/hlsl/pbr.png
gallery:
---              

Trying out different BRDFs, especially classic Blinn-Phong shading model, I've ended up with Physically Based Shading shader.
The gray color is the result of miss-capitalizing "White" instead of "white" in default _MainTex value.

![Shader](../../assets/images/shaders/hlsl/pbr.png)



Shader code:
```glsl
// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

// Upgrade NOTE: replaced 'mul(UNITY_MATRIX_MVP,*)' with 'UnityObjectToClipPos(*)'

Shader "Custom/My First Lightning Shader"
{
    Properties
    {
        _Tint ("Tint", Color) = (1.,1.,1.,1.)
        _MainTex("Albedo", 2D) = "White" {}
        [Gamma] _Metallic("Metallic", Range(0,1)) = 0.
        _Smoothness("Smoothness", Range(0,1)) = .5
    }    
    
    SubShader
    {
        Pass
        {
            Tags {
				"LightMode" = "UniversalForward"
			}
            
            CGPROGRAM

            #pragma target 3.0
            #pragma vertex vert
            #pragma fragment frag

            #include "UnityPBSLighting.cginc"


            float4 _Tint;
            sampler2D _MainTex;
            float4 _MainTex_ST;
            float _Smoothness;
            float _Metallic;

            struct Interpolators
            {
                float4 position : SV_POSITION;
                float2 uv : TEXCOORD0;
                float3 normal : TEXCOORD1;
                float3 worldpos : TEXCOORD2;
            };

            struct VertexData
            {
                float4 position : POSITION;
                float3 normal : NORMAL;
                float2 uv : TEXCOORD0;
                
            };

            Interpolators vert(VertexData v)
            {
                Interpolators i;
                i.uv = TRANSFORM_TEX(v.uv, _MainTex);
                i.position = UnityObjectToClipPos(v.position);
                i.worldpos = mul(unity_ObjectToWorld, v.position);
                i.normal = UnityObjectToWorldNormal(v.normal);
                
                return i;
            }

            float4 frag(Interpolators i) : SV_TARGET
            {
                i.normal = normalize(i.normal);
                float3 lightDir = _WorldSpaceLightPos0.xyz;
				float3 lightColor = _LightColor0.rgb;
                float3 viewDir = normalize(_WorldSpaceCameraPos - i.worldpos);
                float3 reflectionDir = reflect(-lightDir, i.normal);
                float3 albedo = tex2D(_MainTex, i.uv).rgb * _Tint.rgb;
                float3 specularTint;
                float oneMinusReflectivity = 1 - _Metallic;
                albedo = DiffuseAndSpecularFromMetallic(albedo,
                                                    _Metallic,specularTint,
                                                    oneMinusReflectivity);

                UnityLight light;
				light.color = lightColor;
				light.dir = lightDir;
				light.ndotl = DotClamped(i.normal, lightDir);               


                UnityIndirect indirectLight;
				indirectLight.diffuse = 0;
				indirectLight.specular = 0;
                
				return UNITY_BRDF_PBS(albedo,specularTint,oneMinusReflectivity, 
                _Smoothness,i.normal, viewDir,light, indirectLight);
            }
            
            ENDCG
        }        
    }
}
```