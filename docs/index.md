---
layout: default
title: Ana Sayfa
---
## Introduction

Welcome to the Kubernetes notes repository. This document serves as a comprehensive guide for the Certified Kubernetes Administrator (CKA) exam and general Kubernetes knowledge.

## Table of Contents

- [CKA: Certified Kubernetes Administrator Notları](#cka-certified-kubernetes-administrator-notları)
    - [Index](#index)
    - [Bookmarks](#bookmarks)
    - [Cluster Architecture](#cluster-architecture)
    - [Docker ve containerd](#docker-ve-containerd)
    - [CRI, OCI, ImageSpec, RuntimeSpec, rkt, nerdctl, ctl, crictl](#cri-oci-imagespec-runtimespec-rkt-nerdctl-ctl-crictl)
    - [etcd ve etcdctl](#etcd-ve-etcdctl)
    - [Kube API Server](#kube-api-server)
    - [Kube Controller Manager](#kube-controller-manager)
    - [Node Controller](#node-controller)
    - [Replication Controller](#replication-controller)
    - [ReplicaSet](#replicaset)
        - [ReplicaSet Tanımı İçindeki Alanlar ve Olası Değerler](#replicaset-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
        - [Örnek ReplicaSet Yapılandırması ve Alan Seçenekleri](#örnek-replicaset-yapılandırması-ve-alan-seçenekleri)
    - [Kube-Scheduler](#kube-scheduler)
    - [Kubelet](#kubelet)
    - [Kube-proxy](#kube-proxy)
    - [Kubeadm](#kubeadm)
    - [Pod](#pod)
    - [YAML'in Kubernetes ile İlişkisi](#yamlin-kubernetes-ile-i̇lişkisi)
    - [Pod Definition YAML Örnekleri ve Olası Alan Seçenekleri](#pod-definition-yaml-örnekleri-ve-olası-alan-seçenekleri)
        - [Örnek Pod Tanımı (YAML)](#örnek-pod-tanımı-yaml)
        - [Pod Tanımı İçindeki Alanlar ve Olası Seçenekler](#pod-tanımı-i̇çindeki-alanlar-ve-olası-seçenekler)
        - [`containers` Alanındaki Olası Seçenekler](#containers-alanındaki-olası-seçenekler)
    - [Labels ve Selectors](#labels-ve-selectors)
        - [Selectors için Örnek Kullanım Alanları](#selectors-için-örnek-kullanım-alanları)
    - [Deployment](#deployment)
        - [Deployment Tanımı İçindeki Alanlar ve Olası Değerler](#deployment-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
    - [Sertifikasyon İpucu](#sertifikasyon-i̇pucu)
    - [Kubernetes Services](#kubernetes-services)
        - [Service Tanımı İçindeki Alanlar ve Olası Değerler](#service-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
    - [Kubernetes Namespaces](#kubernetes-namespaces)
        - [Namespace Tanımı İçindeki Alanlar ve Olası Değerler](#namespace-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
    - [Resource Quotas (Kaynak Kotaları)](#resource-quotas-kaynak-kotaları)
    - [Role-Based Access Control (RBAC)](#role-based-access-control-rbac)
        - [Özet](#özet)
        - [POD](#pod-1)
        - [Deployment](#deployment-1)
        - [Service](#service)
    - [`kubectl apply` Komutu](#kubectl-apply-komutu)
    - [Manuel Scheduling (Manuel Planlama)](#manuel-scheduling-manuel-planlama)
        - [Açıklama:](#açıklama)
    - [Taints ve Tolerations](#taints-ve-tolerations)
        - [Açıklama:](#açıklama-1)
        - [Açıklama:](#açıklama-2)
        - [A quick note on editing Pods and Deployments](#a-quick-note-on-editing-pods-and-deployments)
            - [Edit a POD](#edit-a-pod)
            - [Edit Deployments](#edit-deployments)
    - [Resource Requests ve Limits](#resource-requests-ve-limits)
    - [DaemonSets](#daemonsets)
        - [Açıklama:](#açıklama-3)
    - [Static Pods](#static-pods)
        - [Açıklama:](#açıklama-4)
  - [CKA: Certified Kubernetes Administrator Notları](#cka-certified-kubernetes-administrator-notları)
    - [Index](#index)
    - [Bookmarks](#bookmarks)
    - [Cluster Architecture](#cluster-architecture)
    - [Docker ve containerd](#docker-ve-containerd)
    - [CRI, OCI, ImageSpec, RuntimeSpec, rkt, nerdctl, ctl, crictl](#cri-oci-imagespec-runtimespec-rkt-nerdctl-ctl-crictl)
    - [etcd ve etcdctl](#etcd-ve-etcdctl)
    - [Kube API Server](#kube-api-server)
    - [Kube Controller Manager](#kube-controller-manager)
    - [Node Controller](#node-controller)
    - [Replication Controller](#replication-controller)
    - [ReplicaSet](#replicaset)
        - [ReplicaSet Tanımı İçindeki Alanlar ve Olası Değerler](#replicaset-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
        - [Örnek ReplicaSet Yapılandırması ve Alan Seçenekleri](#örnek-replicaset-yapılandırması-ve-alan-seçenekleri)
    - [Kube-Scheduler](#kube-scheduler)
    - [Kubelet](#kubelet)
    - [Kube-proxy](#kube-proxy)
    - [Kubeadm](#kubeadm)
    - [Pod](#pod)
    - [YAML'in Kubernetes ile İlişkisi](#yamlin-kubernetes-ile-i̇lişkisi)
    - [Pod Definition YAML Örnekleri ve Olası Alan Seçenekleri](#pod-definition-yaml-örnekleri-ve-olası-alan-seçenekleri)
        - [Örnek Pod Tanımı (YAML)](#örnek-pod-tanımı-yaml)
        - [Pod Tanımı İçindeki Alanlar ve Olası Seçenekler](#pod-tanımı-i̇çindeki-alanlar-ve-olası-seçenekler)
        - [`containers` Alanındaki Olası Seçenekler](#containers-alanındaki-olası-seçenekler)
    - [Labels ve Selectors](#labels-ve-selectors)
        - [Selectors için Örnek Kullanım Alanları](#selectors-için-örnek-kullanım-alanları)
    - [Deployment](#deployment)
        - [Deployment Tanımı İçindeki Alanlar ve Olası Değerler](#deployment-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
    - [Sertifikasyon İpucu](#sertifikasyon-i̇pucu)
    - [Kubernetes Services](#kubernetes-services)
        - [Service Tanımı İçindeki Alanlar ve Olası Değerler](#service-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
    - [Kubernetes Namespaces](#kubernetes-namespaces)
        - [Namespace Tanımı İçindeki Alanlar ve Olası Değerler](#namespace-tanımı-i̇çindeki-alanlar-ve-olası-değerler)
    - [Resource Quotas (Kaynak Kotaları)](#resource-quotas-kaynak-kotaları)
    - [Role-Based Access Control (RBAC)](#role-based-access-control-rbac)
          - [Özet](#özet)
        - [POD](#pod-1)
        - [Deployment](#deployment-1)
        - [Service](#service)
    - [`kubectl apply` Komutu](#kubectl-apply-komutu)
    - [Manuel Scheduling (Manuel Planlama)](#manuel-scheduling-manuel-planlama)
        - [Açıklama:](#açıklama)
    - [Taints ve Tolerations](#taints-ve-tolerations)
        - [Açıklama:](#açıklama-1)
        - [Açıklama:](#açıklama-2)
        - [A quick note on editing Pods and Deployments](#a-quick-note-on-editing-pods-and-deployments)
          - [Edit a POD](#edit-a-pod)
          - [Edit Deployments](#edit-deployments)
    - [Resource Requests ve Limits](#resource-requests-ve-limits)
    - [DaemonSets](#daemonsets)
        - [Açıklama:](#açıklama-3)
    - [Static Pods](#static-pods)
        - [Açıklama:](#açıklama-4)